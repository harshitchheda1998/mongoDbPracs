# Insert Document

## Methods:
* **insertOne({field:value})**
* **insertMany([{field:value},...])**
* **insert({field:value}/[{field:value},{...}])**

## Ordered Insert
Suppose we have a `sports` collection,
```
{ "_id" : "badminton", "name" : "badminton" }
{ "_id" : "racing", "name" : "racing" }
```
with custom `_id`\
Now, if we try to insert 3 new documents with `_id` of `racing` being one of them as a duplicate key
```
db.sports.insertMany([{_id:"swimming",name:"swimming"},{_id:"racing",name:"racing"},{_id:"tennis",name:"tennis"}] )

uncaught exception: BulkWriteError({
        "writeErrors" : [
                {
                        "index" : 1,
                        "code" : 11000,
                        "errmsg" : "E11000 duplicate key error collection: contactData.sports index: _id_ dup key: { _id: \"racing\" }",
                        "op" : {
                                "_id" : "racing",
                                "name" : "racing"
                        }
                }
        ],
        "writeConcernErrors" : [ ],
        "nInserted" : 1,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
}) :

db.sports.find().pretty()
{ "_id" : "badminton", "name" : "badminton" }
{ "_id" : "racing", "name" : "racing" }
{ "_id" : "swimming", "name" : "swimming" }
```
we get an error stating that the key `racing` is a duplicate key\
But the `_id: swimming` got added but, the 2nd and the last document didn't add.\
This is called an ordered insert\
In MongoDB, in ordered insert operation, if an error occurs at any point of document insert, it just aborts the rest of the process. Since in MongoDB during multiple inert, each document is inserted individually and so if suppose the document no. 2 was not able to get inserted because of some error the process stops there and there is no rollback of the insert operation.

But you can change the behavior by just stating the `{ordered: false}` as an optional argument with `insertMany` command
> Note: By default, the `ordered` key is set to true i.e insert in an orderly fashion
```
db.sports.insertMany([{_id:"cricket",name:"cricket"},{_id:"racing",name:"racing"},{_id:"tennis",name:"tennis"}],{ordered : false} )

uncaught exception: BulkWriteError({
        "writeErrors" : [
                {
                        "index" : 1,
                        "code" : 11000,
                        "errmsg" : "E11000 duplicate key error collection: contactData.sports index: _id_ dup key: { _id: \"racing\" }",
                        "op" : {
                                "_id" : "racing",
                                "name" : "racing"
                        }
                }
        ],
        "writeConcernErrors" : [ ],
        "nInserted" : 2,
        "nUpserted" : 0,
        "nMatched" : 0,
        "nModified" : 0,
        "nRemoved" : 0,
        "upserted" : [ ]
})

db.sports.find().pretty()
{ "_id" : "badminton", "name" : "badminton" }
{ "_id" : "racing", "name" : "racing" }
{ "_id" : "swimming", "name" : "swimming" }
{ "_id" : "cricket", "name" : "cricket" }
{ "_id" : "tennis", "name" : "tennis" }
```
If you observe even though there was an error in the 2nd document rest of the documents got inserted

