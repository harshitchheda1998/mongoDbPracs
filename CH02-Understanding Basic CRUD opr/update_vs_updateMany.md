# Difference between update and updateMany
* Let's first see update and updateOne
###### UpdateOne
```
 db.flightData.updateOne({"_id" : ObjectId("606846d3529347e753a86834")},{$set:{delayed:true}} )
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

 db.flightData.find().pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true,
        "delayed" : true
}
{
        "_id" : ObjectId("606846d3529347e753a86835"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}
```
###### Update
    
    
```
 db.flightData.update({"_id" : ObjectId("606846d3529347e753a86834")},{$set:{delayed:false}} )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

db.flightData.find().pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true,
        "delayed" : false
}
{
        "_id" : ObjectId("606846d3529347e753a86835"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}
```

* **Now let's look and updateMany and update**
* update and updateMany work, in the same way, the only catch is that in updateMany and updateOne we need to pass the atomic operator whereas in update it is not necessary
* Let's look at a scenario where we don't pass atomic operator
```
 db.flightData.update({"_id" : ObjectId("606846d3529347e753a86834")},{delayed:false} )
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

db.flightData.find().pretty()
{ "_id" : ObjectId("606846d3529347e753a86834"), "delayed" : false }
{
        "_id" : ObjectId("606846d3529347e753a86835"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}
```
Did you observe the entire object got overwritten with just the `delayed` and the `_id` key value

Now let's look at another method called **replaceOne**
```
db.flightData.find().pretty()
{
        "_id" : ObjectId("60684f8e529347e753a86836"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
{
        "_id" : ObjectId("60684f8e529347e753a86837"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}


db.flightData.replaceOne({"_id" : ObjectId("60684f8e529347e753a86836")},{deplayed: true})
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }

db.flightData.find().pretty()
{ "_id" : ObjectId("60684f8e529347e753a86836"), "deplayed" : true }
{
        "_id" : ObjectId("60684f8e529347e753a86837"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}
```
***So, if you use the atomic operator with update then it works as updateMany else it works as replaceOne***
