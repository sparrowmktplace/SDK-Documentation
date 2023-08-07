---
layout: default
title: Scheduler
nav_order: 7
---

# **Scheduler**

You can create one-time or recurring scheduled events to invoke serverless apps. At the specified time, the App Framework is notified which then invokes the relevant serverless method in the app.

## **Registration**

To register a scheduled event and the corresponding callback:
- From your app’s root directory, navigate to the manifest.json file.
- Include the events attribute, specifying the scheduled event and the corresponding callback methods as follows:

```bash
"events": {
    "onScheduledEvent": {
        "handler": "onScheduledEventHandler"
    }
}
```

- Navigate to the server.js file.

```bash
exports = {
    onScheduledEventHandler: function(payload) {
        console.log("Logging arguments from onScheduledEvent: " + JSON.stringify(payload));
        if (payload.data.account_id === "3") {
            //app logic
        }
    }
};
```

## **Sample payload**

```bash
{
  "account_id" : "1234",
  "eventType"  : "onScheduledEvent",
  "data" : {
    "key":"value"
  },
  "iparams" : {
      "Param1" : "Piadgahkdfa734egrdafdsfsdf",
    }
}
```

## **Schedules**

You can use the following methods to [create one time](#create-one-time-schedule), [recurring](#recurring-schedule), [update](#update-a-schedule), [fetch](#fetch-a-schedule), [delete](#delete-a-schedule), [pause](#pause-a-schedule) and [resume](#resume-a-schedule) schedules.

All of these methids are implemented in server.js

## **Create One Time Schedule**

```bash
try {
    await $Schedule.create({
      name: "sample",
      data: "hello world",
      schedule_at: "2023-06-10T07:00:00.860Z"
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Recurring Schedule**

```bash
try {
    await $Schedule.create({
      name: "sample",
      data: "hello world",
      cronExpression: "0 8 * * *",
      type: 'CRON'
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Fetch a Schedule**

```bash
try {
    await $Schedule.fetch({
      name: "sample"
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Update a Schedule**

```bash
 try {
    await $Schedule.update({
      name: 'sample',
      data: "new data",
      schedule_at: "2023-07-10T07:00:00.860Z"
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Delete a Schedule**

```bash
try {
    await $Schedule.delete({
      name: 'sample'
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Pause a Schedule**

```bash
try {
    await $Schedule.pause({
      name: 'sample'
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

## **Resume a Schedule**

```bash
 try {
    await $Schedule.resume({
      name: 'sample'
    });
  } catch (error) {
    console.log(`Error occured`);
    throw error;
  }
```

Ensure that the onScheduledEvent.json file, which contains the sample payload to test scheduled events, is available at <app's root directory>/server/test_data.