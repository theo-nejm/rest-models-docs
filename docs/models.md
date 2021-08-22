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

_You can - and we recommend to - sequentially set attributes before saving'em instead saving after each change._

### `save()`

Saves the data you modified.

If the data haves a `primaryKey (id, _id, ...)` it will fetch using the put or patch method (based on your preference you've set on model config), else, the data will be fetched with post method.

Basically you will insert some data, modify - if you want to - and save it.

To fetch the data, will be used the url you've set on model's config.

### `get()`

Can receive an optional attribute parameter. In this case, it will return the value stored at this parameter in your object.

```js
myModel.get('name'); // returns 'Jake'
```

If there isn't parameters, get method returns the entire object.

```js
myModel.get(); // returns the entire object
```

### `getOriginalData()`

Returns the full object as how it was before been changed.

If it has never been changed, returns the initial object.

Saved changes can't be retrieved by this method.

```js
myModel.setData({ id: 1, name: 'Kobe' })

myModel.getOriginalData(); // returns the data with name 'Kobe'

myModel.set({ name: 'Paul' })

myModel.getOriginalData(); // returns data with name 'Kobe'

myModel.save(); // saves the data (fetch)

myModel.getOriginalData(); // returns data with name 'Paul'
```

### `remove()`

Makes a delete request based on object's primary key.

If there isn't a primary key in the object, remove method will not execute.

```js
myModel.get() // { id: 1, name: 'Paul' }

myModel.remove() // delete request to api based on id (1)
```

### `hasChanges()`

Returns a boolean based on if there is some unsaved change.

```js
myModel.get() // { id: 1, name: 'Paul' }

myModel.set({ name: 'Ryan' })

myModel.hasChanges() // returns true 

myModel.save()

myModel.hasChanges() // returns false
```

### `getChanges()`

Returns a object of everything has changed in the model before saving it.

```js
const myData = {
    id: 1515,
    name: "Samantha",
    lastName: "Bucks",
    age: 18,
    country: "England",
    gender: "F",
    cellphoneNumber: "999777666"
}

myModel.setData(myData);

myModel.set({ lastName: "Williams", age: 21 });

myModel.set({ lastName: "Jeffrey" });

myModel.getChanges(); // returns { lastName: "Jeffrey", age: 21 }
```

### `discardChanges()`

Discards all unsaved changes and throws back the model to this original form.

As original form we can understand the initial form - if there isn't saves - or last saved object data.

```js
// using the same object of getChanges() example...

myModel.discardChanges(); // discards { lastName: "Jeffrey", age: 21 }

myModel.set({ lastName: "Jeffrey", age: 21 });

myModel.save();

myModel.discardChanges(); // doesn't discard anything

```