# Some basic initial commands for your Information with the help of an example
* Displaying databases\
`show dbs`
* Connecting to a database `shop` which may or may not exist yet\
`use shop`
> Note: The `use` command helps to connect to a database even if it does not exist. If the database does not exist it will be created on a fly once you start inserting data
* Creating a collection of `products` in your connected database on a fly if the collection does not exist yet by simply inserting data inside it
`db.products.insertOne({"name":"A book","price":12.99})`
> Note: You can also insert documents using `db.products.insertOne({name:"A book",price:12.99})` wherein the key does not have quotes where they do get added behind the scenes in MongoDB

You can check if the document was inserted or not from the response which we get from the mongoDB server which contains a unique insertedId
```
{
        "acknowledged" : true,
        "insertedId" : ObjectId("60615fd8a4074046091dbdb6")
}
```
* Find the documents\
`db.products.find()`\
`db.products.find().pretty()`
> Note: `pretty()` helps to display the result in a pretty format
* inserting an embedded document\
`db.products.insertOne({"name":"A computer","price":1229.99, description : "A brand new high quality computer",details:{cpu:"Intel i7", memory : 32}})`