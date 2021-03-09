## Mongoose ToDoList v2
---

Getting Started


1. Lets get our starting files [starting-files-todolistv2](appbrewery.co) // This wasn't working so I just copied my v1 files.

2. If there is no no node_modules folder, like most repo's from the internet, then you will need to npm i to install those depenencies.  // I was up to date
    - You can verify via package.json dependencies



### How to connect our data to a DB?
---

1. We must identify how our data is currenlty being served.
    - Lets remove our current arrays
        ```
        const items = ["Buy Food", "Cook Food", "Eat Food"];
        const workItems = [];
        ```
    
2. Connecting Mongo to our app
    - npm i mongoose // installs mongoose
    - check package.json dependencies to verify
    - Once installed, we can require it next 
        - const mongoose = require("mongoose");
    - Next we can call mongoose.connect and add the depecration warning flags
        - mongoose.connect("mongodb://localhost:27017/todolistDB", {useNewUrlParser: true})


### Challenge | Create a new Items Schema
---

1. Create a new Items Schema  with one field, called name with datatype string.
    - Example Schema 
    ```
    const <schemaName> = {
        <fieldName>: <fieldDataType>
    }
    ```

    - Challenge Solution
    ```
    const itemsSchema = {
    name: String
    }
    ```

### Create a Mongoose Model to work with your Schema
---

- Example Mongoose Model 
    ```
    const ModelName = mongoose.model(    //Capitalize this Const
        <"SingularCollectionName">,
        <schemaName>
    )
    ```



### Remove Day Code
---


const date = require(__dirname + "/date.js");
let day = date.getDay();

