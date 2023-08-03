---
layout: default
title: Data Storage
nav_order: 6
---

# **Data Storage**

Some apps need to store and retrieve data. For example apps like power BI needs to store mapping details of each user. TO achieve this we have key - value data storage where the data of the users are stored as a key-value pair and can also be retrieved. 

For this tutorial let say we have an object:

```bash
const newData = {
    id: 123,
    name: "john doe"
  }
```

- ## **Store**
  
&emsp;&emsp; **app.js**
  - `await client.db.set(key, value)` can be used to set a key-value pair for a user.

  ```bash
  await client.db.set("data", newData);
  ```

&emsp;&emsp; **server.js**
  - `await $Storage.set(key, value)` can be used to set a key-value pair from the server side code of the app.

  ```bash
  await $Storage.set("data", newData);
  ```

- ## **Retrive**
  
&emsp;&emsp; **app.js**
  - `const result = await client.db.get(key)` can be used to retrieve a value for that particular user. It returns a stringified object which can be then converted to json by `JSON.parse(result)`.

  ```bash
  const result = JSON.parse(await window.client.db.get("data"));
  ```

&emsp;&emsp; **server.js**
- `const result = await $Storage.get(key)` can be used to retrieve a value for that particular key for that particular user from the server side code of your app.

```bash
const result = await $Storage.get("data");
```


