* Let's check the `flightData` first
```
> db.flightData.find().pretty()
{
        "_id" : ObjectId("60683cbd529347e753a86832"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
{
        "_id" : ObjectId("60683cbd529347e753a86833"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false
}
```
* Now lets delete 1 document from it
### Delete operation for a Single Document
`db.flightData.deleteOne({departureAirport:"MUC"})`
> Note: In mongoDB filters are also considered dcuments. 
> Note: In mongoDB,For delete/update operation a filter is a required arguement

* Now lets delete multiple documents. For deleteng multiple documents we need a common parameter/criteria, so first lets see updating one/many documents
### Update operation for a Single Document
`db.flightData.updateOne({aircraft:"Airbus A380"},{scraped : true})`
If I try running this query it will an error saying
```
uncaught exception: Error: the update operation document must contain atomic operators :
DBCollection.prototype.updateOne@src/mongo/shell/crud_api.js:565:19
@(shell):1:1
```
Because in MongoDB we cannot simply update a document since mongoDB cannot interpret the new updation to be done Instead we  pass a `$set` operator to let mongoDB know that we want to set the new data to a particular document
> Note: In mongoDB anything whih starts with $ is a reserved keyword/operator
`db.flightData.updateOne({aircraft:"Airbus A380"},{$set:{scraped : true}})`

*  Let's try updating multiple documents
### Update operation for multiple documents
`db.flightData.updateMany({},{$set:{flightCancelled:true}})`
> Note: In Update/delete operation if you pass empty parenthesis for a filter it considers it as update all/ delete all

* Now let's try deleting multiple documents
### Delete operation for multiple documents
`db.flightData.deleteMany({flightCancelled : true})`
* If you want to clear the entire collection/ delete all documents in the collection
`db.flightData.deleteMany({})`