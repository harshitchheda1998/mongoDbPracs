# Cursor Object
* First insert the passenger's data using the file from the resources folder
* Now let's try fetching all the passengers
```
> db.passesngers.find().pretty()
{
        "_id" : ObjectId("606933cf8f3756ec113fe60b"),
        "name" : "Max Schwarzmueller",
        "age" : 29
}
{
        "_id" : ObjectId("606933cf8f3756ec113fe60c"),
        "name" : "Manu Lorenz",
        "age" : 30
}
{
        "_id" : ObjectId("606933cf8f3756ec113fe60d"),
        "name" : "Chris Hayton",
        "age" : 35
}
.
.
.
.
{
        "_id" : ObjectId("606933cf8f3756ec113fe61e"),
        "name" : "Albert Twostone",
        "age" : 68
}
Type "it" for more
```
If you notice that in our data `Albert` was not the last entry to be inserted
If you notice there is something written `Type "it" for more`. Now what does the command `it` in the shell

The answer is cursor Object

## Cursor Object
* The `find()` command no matter on which collection gives you back a Cursor Object and not the entire data
* `find()` does not return an array of documents from a collection and that makes a certain sense because the collection could be very big and if find() immediately sends us all the documents this might take a super long time but also pass a lot of data over the wire.
  So instead of that, it sends us a Cursor Object which contains a lot of meta-data behind which helps us to cycle through the result
* Hence, what the `it` command did is it used the cursor to get the next bunch of data

If you want all the documents use:
```
 db.passesngers.find().toArray()
[
        {
                "_id" : ObjectId("606933cf8f3756ec113fe60b"),
                "name" : "Max Schwarzmueller",
                "age" : 29
        },
        {
                "_id" : ObjectId("606933cf8f3756ec113fe60c"),
                "name" : "Manu Lorenz",
                "age" : 30
        },
        .
	.
	.
        {
                "_id" : ObjectId("606933cf8f3756ec113fe61e"),
                "name" : "Albert Twostone",
                "age" : 68
        },
        {
                "_id" : ObjectId("606933cf8f3756ec113fe61f"),
                "name" : "Gordon Black",
                "age" : 38
        }
]
```
Now what the `toArray()` does is, it just exhausts the cursor to get all the result from every cycle and not stop at the first cycle (i.e 20 documents which is the default size of cursor provided by MongoDB)
So this is not an optimal solution

What you can do and which is mostly done in most of the apps is that you can use `forEach()`
` db.passesngers.find().forEach((passengerData) => {printjson(passengerData)})`
* So what forEach does is, it just loops through the cursor object and brings the data for each cycle and does not load all the data into memory first and then display like `toArray()`

* Also if we see, is that we cannot use `pretty()` with `findOne()`. The reason is that pretty() exists only for cursor Object 
* The cursor object exists only for find and not for insert/update/delete. The reason being that cursor is required only when fetching is required and not when data manipulation is been performed