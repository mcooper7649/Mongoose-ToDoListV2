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



### Create 3 new documents
---

- Example Mongoose Document

const <constantName> = new <ModelName> ({
    fieldName: fieldData
})

```
const item1 = new Item ({
    name: "Welcome to your todolist"
})

const item2 = new Item ({
    name: "Hit the + button to add a new item."
})

const item3 = new Item ({
    name: "<-- Hit this to delete an item."
})

```

### Lets add these documents to an Array
---

``const defaultItems = [item1, item2, item3];``



### Now we can use the insertMany method of mongoose to our items collection
---

- Example Code for Mongoose insertMany()
    - Take note we are accessing the singlular, model Item

<ModelName>.insertMany(<documentArray>, function(err){
    //Deal with error or log success
})

```
Item.insertMany(defaultItems, function(err){
    if (err){
        console.log(err, "Insert Many Unsuccessful")
    }else{
        console.log("Insert Succesful")
    }
})
```

### Now we can test our code
---

1. show dbs from the mongo shell

2. use todolistDB

3. show collections // Should show items collection

4. db.items.find() // This will find all items 



### How do we add find from the app.js?
---

1. We can use the find() method on our Item
    - Item.find({}, function(err, foundItems){
        if (err){
            console.log("Error")
        } else {
            console.log("Success)
        }
    }) 
    - This is a find all with data validation and the returned variable, foundItems in this case, you can be a relative var.


2. We also need to update the app.get with foundItems instead of our old deleted array items.
    - change items to foundItems
    - lets move our res.render into the callback

3. Our Items are loading now but the entire document is being rendered for each list item
    - We only need the name field to be displayed
    - We can access the list.ejs file
        - lets modify newListItem[i] to have .name added.
    - Lets update our for loop to the newer ES6 forEach()

```
<%  newListItems.forEach(function(item){ %>
        <div class="item">
            <input type="checkbox" name="" id="">
            <p><%= item.name %></p>
        </div>
        <% }) %>
```


4. Everytime we run our App.js we re-insert our array. 
    - We can't comment out code so easily of an application while its remotely hosted so we need to add an if statement
    ```
    Item.find({}, function(err, foundItems){
        if (err){
            console.log("Error")
        } else {
            if(foundItems.length === 0){
                Item.insertMany(defaultItems, function(err){
                    if (err){
                        console.log(err, "Insert Many Unsuccessful")
                    }else{
                        console.log("Insert Succesful")
                    }
                })
            }

            res.render("list", {
                listTitle: "Today",
                newListItems: foundItems
            });
        }})
});
    ```


    
    - Lets first drop aka delete our todolist db and to so we can re-insert with the if statement.
        - db  from shell to show what db your connected to
        - db.dropDatabase() from shell deletes

    
```
app.get("/", function(req, res ){
   Item.find({}, function(err, foundItems){
       if(foundItems.length === 0){
           Item.insertMany(defaultItems, function(err){
               if (err){
                   console.log(err);
               } else {
                   console.log("Successfully saved default items to DB");
               }
           })
           res.redirect("/")
       } else {
        res.render("list", {
            listTitle: "Today",
            newListItems: foundItems
        });
       }
   })
})

```


5. Now we need to modify the post route to add a new item to our array.
    - First lets comment out our app.post work route
    - Second we remove the app.post "/" code.
    - Now we need to create a const itemName = req.body.newItem
    - const item = new Item({
        name: itemname
    })
    - item.save() // shortcut to save to db
    - go back to mongo shell and type these commannds to confirm it worked.
        - show dbs
        - use todolistDB
        - db.items.find()
    - Now the item has been saved we can add our res.redirect("/") and it should dispaly our new item in the list.

6. Now we want to delete items when we check the box on the left from our DB.
    - First lets warp a form around our items in the forEach of the list.ejs
    - Fix css by modifying form tag by appending .item class
    - Lets add action and method to our form
    - We don't want to submit our form we want to delete. So lets chagne the action route to "/delete" and create the post route delete in app.js
    - We need to add a name so we can tap into it
        ``<input type="checkbox" name="checkbox" onChange="this.form.submit()">``
    - This will only submit that the checkbox is on or off, not very useful.
        - We need to assign the items id to the value of the checkbox
            value="<%=item._id%>"
        - Use EJS to accomplish this


    7. Now we we can tap into the value of the checkbox and use Mongoose method findByIdAndRemove

    ```
    app.post("/delete", function(req, res){
    const checkItemId = req.body.checkbox
    Item.findByIdAndRemove(checkItemId, function(err){
        if (err){
            console.log("Unsuccessful")
        } else {
            console.log("Sucessfully Deleted")
        }
    })
    })

    ```

    - Make sure to do error handlinga as mongoose doc specify it won't delete if you don't.