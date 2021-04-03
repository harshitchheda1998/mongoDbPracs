* Lets add 2 documents to `flightData` collection
```
 db.flightData.insertMany([
...   {
...     "departureAirport": "MUC",
...     "arrivalAirport": "SFO",
...     "aircraft": "Airbus A380",
...     "distance": 12000,
...     "intercontinental": true
...   },
...   {
...     "departureAirport": "LHR",
...     "arrivalAirport": "TXL",
...     "aircraft": "Airbus A320",
...     "distance": 950,
...     "intercontinental": false
...   }
... ])
```
> If you see for adding multiple documents we pass an array of documents rather than single documents separated by a comma
 * Let's check the response from the server
```
{
        "acknowledged" : true,
        "insertedIds" : [
                ObjectId("60683cbd529347e753a86832"),
                ObjectId("60683cbd529347e753a86833")
        ]
}
```
MongoDB keeps the order of the insert operation which we can see in the `insertedIds`