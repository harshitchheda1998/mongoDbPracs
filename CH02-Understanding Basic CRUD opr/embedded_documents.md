# Embedded Document
* MongoDB offers up to 100 levels of nesting inside a document
* MongoDB offers up to 16 MegaBytes of size per document
```
{
        "_id" : ObjectId("6068504d529347e753a86839"),
        "departureAirport" : "LHR",
        "arrivalAirport" : "TXL",
        "aircraft" : "Airbus A320",
        "distance" : 950,
        "intercontinental" : false,
        "pilotNames" : [
                "Mark",
                "Jacob"
        ],
        "altitude" : [
                150000,
                "kms"
        ],
        "radarReadings" : [
                117,
                117.5
        ],
        "details" : {
                "description" : "The morning flight to TXL"
        },
        "aircraftPartDetails" : [
                {
                        "part" : "wheels",
                        "number" : 8
                },
                {
                        "part" : "cockpit",
                        "number" : 1
                }
        ]
}
```
You can also store an array of strings like `pilotNames` or an array of numbers like `radarReadings` or an array of different dataType together like `altitude`.\
You can also store an array of embedded documents as well

# Accessing Embedded Document using above example
1. Get the pilot names for aircraft name as "Airbus A320"\
`db.flightData.findOne({"aircraft" : "Airbus A320"}).pilotNames`
> Note: When accessing a particular key make sure you use `findOne` since accessing a key works only with `findOne` and not `find`

2. Get the list of flights where "Mark" is a pilot\
`db.flightData.find({pilotNames:"Mark"}).pretty()`

3. Get the list of flights where the flight is a morning flight to TXL\
`db.flightData.find({"details.description":"The morning flight to TXL"}).pretty()`
> Note: When filtering using inner keys make sure you use quotes and a dot`.` operator to show the nesting

4. Get the list of flights with 8 wheels\
` db.flightData.find({"aircraftPartDetails.part":"wheels","aircraftPartDetails.number":8}).pretty()` 


