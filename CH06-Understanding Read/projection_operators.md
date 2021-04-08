# Projection

Let's use `movies` collection

* Get only the name, genres, runtime and the rating of all movies\
`db.movies.find({},{name:1, genres:1 , rating:1 , runtime : 1}).pretty()`

* Get only only the name, genres, runtime, rating and the schedule time of all movies\
` db.movies.find({},{name:1, genres:1 , rating:1 , runtime : 1, "schedule.time":1}).pretty()`
Here in schema `schedule` is a embedded document with `time` as one key and days as another key which is an array which we are excluding.

# `$`-Positional projection operator
* Get all movies where genres are Thriller and project only Thriller as the name, along with name
`db.movies.find({genres : "Thriller"},{ name:1, "genres.$":1})`

Suppose we need movies where genres are Thriller and Drama and there if I use $ operator for getting the name of genre 
` db.movies.find({genres : {$all : ["Thriller", "Drama"]}},{ name:1, "genres.$":1})`
we get all movies that match the filter but the genre name that appears is only Drama even though it might contain thriller as a genre

# `$elemMatch` projection operator
Suppose you want to get all movies where the genre is drama but display the genre column for those who have thriller as well and for the rest don't display the column then use `$elemMatch` 

```
db.movies.find({genres : "Drama"}, {genres : {$elemMatch : {$eq: "Thriller"}}})
{ "_id" : ObjectId("606d98ec2a05253e78795bc2"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bc3") }
{ "_id" : ObjectId("606d98ec2a05253e78795bc4") }
{ "_id" : ObjectId("606d98ec2a05253e78795bc5") }
{ "_id" : ObjectId("606d98ec2a05253e78795bc6"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bc8"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bc9") }
{ "_id" : ObjectId("606d98ec2a05253e78795bca"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bcb") }
{ "_id" : ObjectId("606d98ec2a05253e78795bcc") }
{ "_id" : ObjectId("606d98ec2a05253e78795bcd") }
{ "_id" : ObjectId("606d98ec2a05253e78795bce") }
{ "_id" : ObjectId("606d98ec2a05253e78795bcf") }
{ "_id" : ObjectId("606d98ec2a05253e78795bd0"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bd2") }
{ "_id" : ObjectId("606d98ec2a05253e78795bd3") }
{ "_id" : ObjectId("606d98ec2a05253e78795bd4"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bd5"), "genres" : [ "Thriller" ] }
{ "_id" : ObjectId("606d98ec2a05253e78795bd6") }
{ "_id" : ObjectId("606d98ec2a05253e78795bd9") }
```
If we see here only those movies which had thriller as a genre alongside drama have genre column displayed and for rest, it is not displayed if they have drama as a genre but not a thriller.
This comes in handy when you cant use $ operator standalone.

# `$slice` projection operator

* Get all movies where genre is drama and fetch only first 2-types of genres from each movies if it cntains multiple genres\
`db.movies.find({genres : "Drama"}, {genres : {$slice : 2}}).pretty()`
Here  `$slice` slices the array to get only the first 2 genres 

`db.movies.find({genres : "Drama"}, {genres : {$slice : [1,2]}}).pretty()`
Here `$slice` slices the array to get only 2 genres by skipping the genre and  first position

