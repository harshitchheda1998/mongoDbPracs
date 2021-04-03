### Finding Documents based on filtering condition
```
db.flightData.find({intercontinental:true})
{ "_id" : ObjectId("606846d3529347e753a86834"), "departureAirport" : "MUC", "arrivalAirport" : "SFO", "aircraft" : "Airbus A380", "distance" : 12000, "intercontinental" : true }
```
* You can also beautify the output using `pretty()`
```
db.flightData.find({intercontinental:true}).pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
```
* You can also filter the output based on some numeric values
```
db.flightData.find({distance : 12000}).pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
```
* You can also filter based on numeric comparisons
```
db.flightData.find({distance : {$gt: 10000}}).pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
```
> Note: Here if you see `db.flightData.find({distance : {$eq: 12000}}).pretty()` & `db.flightData.find({distance : 12000}).pretty()` give the same result for equality
```
db.flightData.find({distance : {$gt: 900}}).pretty()
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
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
* For the above result if you want only a single document then
```
db.flightData.findOne({distance : {$gt: 900}})
{
        "_id" : ObjectId("606846d3529347e753a86834"),
        "departureAirport" : "MUC",
        "arrivalAirport" : "SFO",
        "aircraft" : "Airbus A380",
        "distance" : 12000,
        "intercontinental" : true
}
```
> Note: You cannot use pretty with `findOne` and we will discuss this in the cursor object part later
