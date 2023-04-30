---
layout: default
title: Batch App
nav_order: 8
---

# **Batch app**

The Batch App is for performing tasks that will take more time to complete a repetitive series of sub tasks that will require a server. We can achieve this by using the Function chaining method.

 Let us consider an example, if we want to read contacts from a file and create contacts in SurveySparrow. In this case, consider if there are like 1000 contacts or 10000 contacts, it is a time-consuming task that requires a server. Let us elaborate and break down in steps.

 If we want to read contacts from a csv file, which is in S3, and create contacts in SurveySparrow using public APIs we can do as follows.

 We will write Batch using three functions as follows:

 1. Function 1 ->  send payload and necessary parameters as payload to function 2.

 2. Function 2 -> Read a file from S3, perform chunking on the contacts, send chunked data as payload to function 3.

 3. Function 3 -> Write data in SurveySparrow using public APIs.

 Note : Read SurveySparrow developer [docs](https://developers.surveysparrow.com/rest-apis) to knowing more about public APIs and other types of data you can import and export,  to and from SurveySparrow.

 ```js
 const importHandler = async (payload, $Fetch, $File, $Next, $Batch, $Status) => {
  return (await $Batch.initialize(function1,function2,function3));
};
```

importHandler is the initial function where control begins. Here, we have to initialize Batch app as above with three functions (function names need not be mandatorily the same). All three functions should be written inside importHandler function before return statement.

```js
const function1 = async (payload)=>{
       try{
           await $Next.call(payload.payload, "function2");
           return {status:200};
       }catch(error){
           console.log('Error in function1', error);
           return { status: 500, error };
       }
   };
   ```


Function 1 just needs to send the payload to function 2 using $Next and the second parameter should be necessarily the keyword “function2” for calling the 2nd function.

```js

const function2 = async (payload) => {
   try {
       const contacts = await $File.read(mainPayload.payload.data.key, true, payload.traceId);
       let batchSize =20;
       if(contacts.length <=20){
           batchSize = contacts.length;
       }
       let j=0;
       for (let i = 0; i < contacts.length;) {
           const chunkedData = contacts.slice(i,i+batchSize);
           await $Next.call({data: chunkedData, batch: j}, "function3", );
           i=i+batchSize;
           j++;
       }
       return { status: 200 };
   } catch (error) {
       console.log('Error while chunking in function2', error);
       throw error;
   }
};

```

Now control will come to the 2nd function. Here, we are reading file from S3 using $File.read with three parameters namely key, true, traceID. The first parameter’s object format changes as per the payload we are sending, the rest are same as per sample code.

 After that, here we are setting a batch size as 20, please try to use the same, increasing the batch size more than 20 will cause issues. 
Next we are using a for loop to chunk and split the data like 20 per batch and sending it as payload to function 3 with batch count as well. Here also we are using the $Next.call to call third function and “function3” should be constant and not to be changed.

```js

const function3 = async (payload) => {
   const contact = payload.payload.data.data;
   const batch = payload.payload.data.batch;
   const token = mainPayload.iparams.surveysparrow_api_key;
   const traceId = payload.traceId;
   try {
       const config = {
           headers: {
               Authorization: `Bearer ${token}`
           }
       };
     
       await Promise.all(contact.map(async (item, index) => {
                 const result = await $Fetch.post('https://api.surveysparrow.com/v3/contacts', item, config);
                 let j=index+1;
                await $Status.status(result, j, batch, traceId);
         }));
      
       return {status: 200};
   } catch (error) {
       console.log("Error while writing data for payload", payload.payload.data, error);
        throw { status: 500, error };
   }
};
```

In function3 we are writing data into SurveySparrow. Here we are using Promise.all in order to maintain order of writing data(mandatory to be followed). The $Status is used to set status of the current api calls. It can be used if we want to use the Status feature, by using which , the number of successfully ran and failed status of individual contacts during bulk imports.



 
