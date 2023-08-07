---
layout: default
title: Features
nav_order: 5
---

# **Features**


#### 1. [App Lifecycle Methods](#app-lifecycle-methods)
#### 2. [Data Methods](#data-methods)
#### 3. [Interface Methods](#interface-methods)
#### 4. [Request Methods](#request-method)
#### 5. [OAuth](#oauth)



## **App Lifecycle Methods:**

window.app.initialized():

&emsp;&emsp;The method is triggered when the page that contains the app is loaded for the first time. On success, it returns the client object. As the app renders within an IFrame, all communication (through Data methods, Interface methods, Events methods, Request methods) between the app and parent page occurs through the client object.

Sample client object initialization:

```js
useEffect(() => {
  window.client = window.app.initialized();
}, []);
```

The object is loaded to the window object of the app.

## **Data Methods:**

Currently the support is for only one data method which can be only used for the apps rendered on a survey's integration page(builder_integrations_list).
It can be used like:  <br/> `window.client.data.get(“getSurveyId”)-> returns a long value`

## **Interface Methods:**

You can use the interface methods to trigger certain actions on the SurveySparrow user interface.

Success and failure toaster:

Once the client object is loaded to the window object of the app, then interface method for failure and success toastr can be used like:

`window.client.interface.alertMessage(message, options);` where message is a string and options is a object with key type and value with one of “success” or “failure”.

Sample:

```js
window.client.interface.alertMessage("File upload failed", {
  type: "failure",
});
```

## **Request Method:**

Request method is a secure mechanism to send HTTP requests from an app to a third-party domain, without exposing any sensitive information that is part of the request. Through browsers, app users may intercept sensitive information such as API keys or user credentials. Request method safeguards against such exposure. Typically, HTTP requests are routed through proxy servers. In such cases, sending the HTTP requests through the request method helps identify the origin in scenarios where the third-party domain has enabled Cross Origin Resource Sharing (CORS) support.

In this request method, the hosts to which the apps are expected to make request calls are whitelisted in manifest.json. The other attributes such as methods, paths, query parameters, header parameters, options, and request body are dynamically generated during runtime.

The request Method can be used like:

`const response = await window.client.request.get(“URL”, options, params);`
`const response = await window.client.request.post(“URL”, options, body);`
`const response= await window.client.request.delete(“Url”, options, body);`
The response will be stringified to get the response in json format, it needs to be await `json.parse(response);`

As of now, get, post and delete request methods are supported.

Sample of request Method:

`const domain = “https://api.surveysparrow.com”;`

GET:

in app.js
```js
const result = await window.client.request.get(`${domain}/v3/surveys`, {
     options: {
       headers: {
         Authorization: "Bearer <%=iparams.surveysparrow_api_key%>"
       }, isOAuth: false, maxAttempts: 5
     }
   });
```

in server.js
```js
const result = await $Fetch.get(`${domain}/v3/surveys`, {
       headers: {
         Authorization: "Bearer <%=iparams.surveysparrow_api_key%>"
       }, isOAuth: false, maxAttempts: 5
  })
```

DELETE:

in app.js
```js
 window.client.request.delete(
  `https://www.googleapis.com/calendar/v3/calendars/primary/events/${event.id}`,
  {
    options: {
      headers: {
        Authorization: "Bearer <%= access_token%> 
      },
      isOAuth: true,
      maxAttempts: 5,
    },
  }
);
```

POST:

in app.js
```js
window.client.request.post("https://www.googleapis.com/calendar/v3/calendars/primary/events",
{options:
      {headers:{
         Authorization: "Bearer <%= access_token%> "
       },isOAuth:true,maxAttempts:5}},state)


The response body can be used like:
(JSON.parse(data).body)
```

in server.js
```js
$Fetch.post("https://www.googleapis.com/calendar/v3/calendars/primary/events",state,
  {
        headers:{
         Authorization: "Bearer <%= access_token%> "
       },isOAuth:true,maxAttempts:5
  });
```

## **Status Codes:**

| Code | Description|
|----| :-----------:|
|400 | Is returned due to invalid input. For example, if you are making a request to a domain that has not been whitelisted or if the request is passing secure iparams in the body or URL, you will receive this status.|
|401| Is returned if you performed an unauthorized request.|
|429| Is returned when the number of requests exceeds the threshold.|
|500| Is returned if the server encountered an unexpected error.|
|502| Is returned when there is a network error.|
|503| Is returned when the service is temporarily unavailable.
|504| Is returned when there is a timeout error while processing the request.| 

## **OAuth:**

You can use the **OAuth 2 protocol** to authorize an app to access resources from third-party applications. This is done by using an access token obtained from the third-party when the app is installed.

- Register your app in the third-party AppNest. Once registered, you will be issued with **client_id** and **client_secret** to perform OAuth handshake with the provider.

- Provide the redirect URL for your app in the third-party AppNest.

&emsp;&emsp; 1. Testing:`https://localhost:30001/auth/callback?callback=https://localhost:30001/custom_configs?product=surveysparrow&product=surveysparrow`(the product should be the name of the environment you are testing)

&emsp;&emsp; 2. Production: `https://marketplace.surveysparrow.com/api/marketplace-redirect-url`

| FIELD | DESCRIPTION |
| ----- | :-----------: |
| client_id<br/>**Mandatory** | Once you register your app in the third-party AppNest, you will be issued a client ID for your app.|
|client_secret<br/>**Mandatory** | Once you register your app in the third-party AppNest, you will be issued a client secret for your app.|
|authorize_url<br/>**Mandatory**| Third-party authorization request URL.|
|token_url<br/>**Mandatory** | Request URL for the access token.|
|options | The options field can be used to send:<br/> - Additional parameters to the resource owner while fetching the access token. Custom headers as part of the token phase as required by certain third-party services. For this, custom headers can be included in the options field as an object.<br/> `"customHeaders": { "Authorization" : "Basic API_KEY" }`
|scopes<br/>**(Mandatory only when the app is published in production account. Otherwise the scopes can be added as scope array inside options itself for local testing)** | To control the level of access on the resource. It should be array of strings |
|redirect_url| For production,<br/>`https://marketplace.surveysparrow.com/api/marketplace-redirect-url`

**Note:** while testing the app, the scopes should be given inside options but in production the scopes should be given seperately as scopes.

The access token and refresh token are stored and maintained by the AppNest itself and to use them in the request method’s url and options like: `<%=access_token%>.`

Sample:

in app.js
```js
window.client.request.post("https://www.googleapis.com/calendar/v3/calendars/primary/events",{
  options:{
    headers:{
         Authorization: "Bearer <%=access_token>"
       },isOAuth:true,maxAttempts:5
      }
  },state);

```

in server.js
```js
$Fetch.post("https://www.googleapis.com/calendar/v3/calendars/primary/events",state,{
    headers:{
         Authorization: "Bearer <%=access_token>"
       },isOAuth:true,maxAttempts:5
  })
```

