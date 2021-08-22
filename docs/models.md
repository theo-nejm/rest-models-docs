---
sidebar_position: 2
title: Working with models
---

# Models

Models are abstractions to things you do frequently, like:

- HTTP post, delete, patch/put, get requests.
- Manage the data received from API.
- Compare and make data changes.

## 🌐 Configuring the Model

Models have a very simple configuration.
The config object basically is:

| Name       | Type            | Description                                                                                                                      |
| ---------- | --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **url\***  | string          | The endpoint that represents your object in API route.                                                                           |
| primaryKey | string          | It is used to define the name of the primary key in your object. If received other than 'id'.                                    |
| editMethod | "put" / "patch" | The method that you preffer to use to edit thing in the database. It can only be set with "put" or "patch". Its defaultly "put". |

**Example:**

```js
{
  url: "/users",
  primaryKey: "_id",
  editMethod: "patch"
}
```

## 🪚 Building your model

Your models should be instances of Model class. To instance it, you just have to:

```ts
import { Model } from "rest-models";

const config = {
  url: "/model",
};

const myModel = new Model(config);
```

## 🌎 Populating your model

A model itself can't do anything. For make you model being useful, you have to populate it.
You just have to use the method `setData(data)` and pass as parameter the object that you want to model.

```ts
myModel.setData({
  _id: "1da8sj-fasnhjok3da6v",
  name: "John",
  lastName: "Jeffrey",
  age: 32,
});
```

_The object can - and we recommend to - be fetched from the API you are using in your application._

## 💎 Getting started with the methods

### `set()`

Lets you to modify existing attributes separatedly.<br />

```js
myModel.set({ name: "Jake" });
```

When you do this, your object with new data is on hold, waiting to be saved.

_You can - and we recommend to - sequentially set attributes before saving'em._

### `save()`

Saves the data that you did modifiy.

If the data haves a `primaryKey (id, _id, ...)` it will fetch using the put or patch method (based on your preference you've set on model config), else, the data will be fetched with post method.

Basically you will insert some data, modify - if you want to - and save it.

To fetch the data, will be used the url you've set on model's config.

### `get()`

Can receive an optional attribute parameter. In this case, it will return the value stored at this parameter in your object.

```js
myModel.get("name"); // returns 'Jake'
```

If there isn't parameters, get method returns the entire object.

```js
myModel.get(); // returns the entire object
```

### `getOriginalData()`

Returns the full object as how it was before been changed.

If it has never been changed, returns the initial object.

Saved changes can't be retrieved by this method.

### `remove()`

Makes a delete request based on object's primary key.

If there isn't a primary key in the object, remove method will not execute.
