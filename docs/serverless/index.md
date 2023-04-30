---
layout: default
title: Serverless
nav_order: 7
---

The survey sparrow app development platform includes a serverless environment to enable you to create apps that run in response to events such as survey sparrow product, app setup, and external events. Serverless computing involves servers, but they are abstracted away from developers.

To use this feature, all you need to do is configure an event listener and the callback method. When the event occurs, the callback method is executed on a server. 


Whenever an event happens in the server of the survey sparrow then that event is captured and sent to the app. With this payload the app can do whatever it wants.

In order to capture and use this event the app has to register what are all events that it is using in the `manifest.json` file.

Under the product name:

```json
"events": {
        "<eventName>": {
            "handler": "<eventCallbackMethod>"
            },
            "<eventName>": {
            "handler": "<eventCallbackMethod>"
                }
            }
```

Sample:
```json
  "events":{
"onSubmissionComplete":{
"handler":"submissionHandler"
        },
```
In this, whenever a submission has happened in survey sparrow then that payload will be passed to the app. In the server.js the mentioned handler will get the payload of the submission.

For now, only `onSubmissionComplete` is supported in serverless events.

## **Server Method Invocation:**

The Server Method Invocation (SMI) feature enables you to build an app with a front-end component that can invoke a serverless component. To do this:

1. In the manifest.json > functions object, specify all the server methods that are called from the front-end component of the app, to allowlist the methods.

2. In the front-end component (app.js), specify the method to invoke the serverless component and pass an appropriate payload to the serverless component. By default, the serverless environment adds the installation parameters set during app installation to the payload.

3. In the serverless component (server.js), define the server method (SMI function) that is allow-listed in the app manifest and called from the front-end component. In this server method, include the app logic that runs based on the payload passed and the method will return success and failure responses to the front-end component.


The SMI functions should be mentioned under the productName in manifest.json like:

```json
"functions": {
        "serverMethod1": {
          "timeout": 10
        },
        "serverMethod2": {
          "timeout": 15
        }
      }
```

Sample:
```json
"functions":{
"surveyConverter":{       
    "timeout":10
   }
}
```

The server method can be invoked from the frontend using the client object like:

`window.client.request.invoke(functionName, data);`

Sample:

`const result = await window.client.request.invoke(“surveyConverter”, {data:”sample_data”});
`

After the app logic in the server method runs, the server method sends an appropriate response to the front-end component. To enable this:

- Navigate to the `server.js` file. In the exports code block, define the server method (SMI function) that is called from the front-end component. Place the app logic inside the server method.

The frontend component will get the value retired from this smi function and this value is stringified. The actual JSON can be taken from the retired value by `JSON.parse(returned_value)` in the frontend Component.







