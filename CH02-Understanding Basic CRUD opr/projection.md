# Why do we need projection?
Imagine that we have passengers documents, but for our app, we don't need every information from each document but just the name of the passenger\
We can do this by simply getting all the data and then filtering it out as per our need.\
But even if we need only certain information even then we are passing a lot of data over the wire first which is not optimal.\
Here's where projection helps. We can project only needed information to be passed over the wire at the server itself to reduce the bandwidth of data

```
db.passesngers.find({},{name : 1})
{ "_id" : ObjectId("606933cf8f3756ec113fe60b"), "name" : "Max Schwarzmueller" }
{ "_id" : ObjectId("606933cf8f3756ec113fe60c"), "name" : "Manu Lorenz" }
{ "_id" : ObjectId("606933cf8f3756ec113fe60d"), "name" : "Chris Hayton" }
{ "_id" : ObjectId("606933cf8f3756ec113fe60e"), "name" : "Sandeep Kumar" }
{ "_id" : ObjectId("606933cf8f3756ec113fe60f"), "name" : "Maria Jones" }
{ "_id" : ObjectId("606933cf8f3756ec113fe610"), "name" : "Alexandra Maier" }
{ "_id" : ObjectId("606933cf8f3756ec113fe611"), "name" : "Dr. Phil Evans" }
{ "_id" : ObjectId("606933cf8f3756ec113fe612"), "name" : "Sandra Brugge" }
{ "_id" : ObjectId("606933cf8f3756ec113fe613"), "name" : "Elisabeth Mayr" }
{ "_id" : ObjectId("606933cf8f3756ec113fe614"), "name" : "Frank Cube" }
{ "_id" : ObjectId("606933cf8f3756ec113fe615"), "name" : "Karandeep Alun" }
{ "_id" : ObjectId("606933cf8f3756ec113fe616"), "name" : "Michaela Drayer" }
{ "_id" : ObjectId("606933cf8f3756ec113fe617"), "name" : "Bernd Hoftstadt" }
{ "_id" : ObjectId("606933cf8f3756ec113fe618"), "name" : "Scott Tolib" }
{ "_id" : ObjectId("606933cf8f3756ec113fe619"), "name" : "Freddy Melver" }
{ "_id" : ObjectId("606933cf8f3756ec113fe61a"), "name" : "Alexis Bohed" }
{ "_id" : ObjectId("606933cf8f3756ec113fe61b"), "name" : "Melanie Palace" }
{ "_id" : ObjectId("606933cf8f3756ec113fe61c"), "name" : "Armin Glutch" }
{ "_id" : ObjectId("606933cf8f3756ec113fe61d"), "name" : "Klaus Arber" }
{ "_id" : ObjectId("606933cf8f3756ec113fe61e"), "name" : "Albert Twostone" }
```
Here what we are doing, we are passing `{}` filter to fetch all document and for projection, we are using `{name : 1}` which means include name field from the document
> 1 -> include, 0 -> exclude
* If you observe `_id` was also included even though we didn't mention it. The reason being that `_id` is always included by default
* If you don't want the `_id` field you need to explicitly exclude it
`db.passesngers.find({},{name : 1, _id: 0})`
The best thing is that all the projection is done on the server itself before shipping the data which helps to over bandwidth exhaustion.