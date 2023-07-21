---
layout: default
title: Data Storage
nav_order: 6
---

# **Data Storage**

Some apps need to store and retrieve data. For example apps like power BI needs to store mapping details of each user. TO achieve this we have key - value data storage where the data of the users are stored as a key-value pair and can also be retrieved. 

- `await client.db.set(key, value)` can be used to set a key-value pair for a user.

- `const result = await client.db.get(key)` can be used to retrieve a value for that particular user. It returns a stringified object which can be then converted to json by `JSON.parse(result)`.


