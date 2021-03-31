# Creating database `flights` implicitly (on demand)

* `use flights` is used to switch to the database you need, even if does not exist but the catch is that if you now check the list of databases using `show dbs` you'll notice that the database name does not appear
* The reason for this is that when you try to create a database implicitly it does not get created until you add some data/documents inside it

* The same goes for collections as well if you want to create a collection implicitly.

* So now lets try adding some flight data :
```
db.flightData.insertOne({
...     "departureAirport": "MUC",
...     "arrivalAirport": "SFO",
...     "aircraft": "Airbus A380",
...     "distance": 12000,
...     "intercontinental": true
...   })
```
* Once data gets inserted you get the response with the server whether it was successful or not
* If it gets successful, you receive a unique id for that document
```
{
        "acknowledged" : true,
        "insertedId" : ObjectId("60634d7a7ac6c6fe5f8bb109")
}
```
and now if you search for the document using your `find()` command, you will notice a new `_id` field attached to your document
```
{
        "_id" : ObjectId("60634d7a7ac6c6fe5f8bb109"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
```

* As you can see the ObjectId is not of a valid JSON format but since MongoDB drivers convert the JSON document to BSON document it can recognize it

* Also if you don't want to have an automatic unique id value you can also pass your unique id using `_id` key in your document to be inserted
> Note: the custom `_id` value but be unique for insertion\
> Note: you can also pass the key with the quotes when inserting using the shell as long as the key does not contain any white-spaces\
> Note: the value in the `_id` field is case-sensitive which means `_id:"MUC-NY-1"` is different from `_id:muc-ny-1` while inserting or fetching\

# Some basic CRUD operations :
## Create (C)
* `insertOne(data,options)` for inserting one docuement
* `insertMany(data,options)` for inserting mutiple documents

## Read(R)
* `find(filter,options)` reading a bunch of docuemnts
* `findOne(filter,options)` fetching the first document 

## Update(U)
* `updateOne(filter,data,options)` for updating one document 
* `updateMany(filter,data,options)` for updating multiple documents
* `replaceOne(filter,data,options)` for replacing one document entirely with new one

## Delete(D)
* `deleteOne(filter,options)` deleteing the first docuemnt
* `deleteMany(filter,options)` deleting multiple documents