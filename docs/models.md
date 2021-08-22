---
sidebar_position: 2
title: Working with models
---

# Models

### What are models?

+ Models are abstractions to things you do frequently, like:
    
    + HTTP post, delete, patch/put, get requests.

    + Manage the data received from API.

    + Compaire and make data changes.

### 🌐 Configuring the Model

+ Models have a very simple configuration.

    + The config object basically is: 

    `url: required` <br />
    The endpoint that represents your object in API route. <br />
    _{ url: '/users' }_

    `primaryKey: optional` <br />
    It is used to define the name of the primary key in your object. If received other than 'id'. <br />
    _{ primaryKey: '\_id' }_

    `editMethod: optional` <br />
    The method that you preffer to use to edit thing in the database. It can only be set with "put" or "patch". Its defaultly "put". <br />
    _{ editMethod: 'patch' }_

    ```
    {
        url: '/users',
        primaryKey: '_id',
        editMethod: 'patch'
    }
    ```

### 🪚 Building your model:

+ Your models should be instances of Model class. To instance it, you just have to:

```ts
const myModel = new Model(modelConfigs) // and import it from rest-models

myModel.setData(myData);
```

### 🌎 Populating your model:

+ A model itself can't do anything. For make you model being useful, you have to populate it.

+ You just have to use the funcion myModel.setData(data) and pass as parameter the object that you want to model.

```ts
const config = {
    url: '/users',
    primaryKey: '_id',
}

const userModel = new Model(config);

const myObject = {
    _id: '1da8sj-fasnhjok3da6v',
    name: 'John',
    lastName: 'Jeffrey',
    age: 32,
}

userModel.setData(myObject);
```

+ The object can - and we recommend to - be fetched from the API you are using in your application.

### 💎 Getting started with the methods:

+ set():

    + lets you to modify existing attributes separatedly.<br />
    `userModel.set({ name: 'Jake' })`

    + When you do this, your object with new data is on hold, waiting to be saved.

    + You can - and we recommend to - sequentially set attributes before saving'em.

+ save():
    
    + saves the data that you did modifiy.
    
    + If the data haves a primaryKey (id, _id, etc) it will fetch using the put or patch method (based on your preference you've set on model config), else, the data will be fetched with post method.

    + Basically you will insert some data, modify - if you want to - and save it. 

    + To fetch the data, will be used the url you've set on model's config.

+ get():
    
    + can receive an optional attribute parameter. In this case, it will return the value stored at this parameter in your object.

    `userModel.get('name') // returns 'Jake'`

    + if there isn't parameters, get method returns the entire object.

    `userModel.get() // returns the entire object`

+ getOriginalData():
    
    + returns the full object as how it was before been changed.

    + if it has never been changed, returns the initial object.

    + saved changes can't be retrieved by this method.


+ remove():

    + makes a delete request based on object's primary key.

    + if there isn't a primary key in the object, remove method will not execute.
