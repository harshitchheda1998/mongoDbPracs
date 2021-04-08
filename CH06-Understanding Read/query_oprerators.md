Let's use the `movies` collection which we imported
# Working with comparison operators

* All movies with runtime of 60\
`db.movies.find({runtime: 60}).pretty()` or `db.movies.find({runtime: {$eq : 60}}).pretty()`

* All movies with runtime not equal to 60\
` db.movies.find({runtime: {$ne : 60}}).pretty()`

* All movies with runtime less than 60\
` db.movies.find({runtime: {$lt : 60}}).pretty()`

* All movies iwth  average rating gt 7\
`db.movies.find({"rating.average":{$gt : 7}}).pretty()` in the schema rating is an embedded document

* All moveis with one of the genres as drama\
`db.movies.find({genres:"Drama"}).pretty()` in the schema genres is an array

* All movies with genres as on only drama\
`db.movies.find({genres:["Drama"]}).pretty()`

* All movies with runtime of 30 or 40\
` db.movies.find({runtime:{$in : [30, 42]}}).pretty()`
> Note : `$in` takes an array of values to match for

* All movies with runtime  not of 30 or 40\
` db.movies.find({runtime:{$nin : [30, 42]}}).pretty()`
> Note : `$nin` takes an array of values not to match for

# Working with logical operators

* All movies with average rating greater than 9.3 or less than 5\
`db.movies.find({$or: [{"rating.average":{$lt:5}},{"rating.average":{$gt:9.3}}]}).pretty()`
> Note: Here unlike comparison operators `$or` is directly started and then it takes an array of multiple conditions within

* All movies with average rating neither greater than 9.3 nor less than 5\
`db.movies.find({$nor: [{"rating.average":{$lt:5}},{"rating.average":{$gt:9.3}}]}).pretty()`
> Note: Here unlike comparison operators `$nor` is directly started and then it takes an array of multiple conditions within

* All movies with average rating neither greater than 9.3 nor less than 5\
`db.movies.find({$nor: [{"rating.average":{$lt:5}},{"rating.average":{$gt:9.3}}]}).pretty()`
> Note: Here unlike comparison operators `$nor` is directly started and then it takes an array of multiple conditions within

* All movies with average rating greater than 9 and one of the genres as drama\
`db.movies.find({$and: [{"rating.average":{$gt:9}},{genres: "Drama"}]}).pretty()`
or
` db.movies.find({"rating.average":{$gt:9}, genres: "Drama"}).pretty()`

* All movies with not runtime of 60\
`db.movies.find({runtime:{$not:{$eq:60}}}).pretty()`
or
`db.movies.find({runtime:{$ne:60}})`
> Note: Here unlike other logical operator $not is not started directly since it is a unary operator

# Working with Element operators
* All movies where runtime exists\
`db.movies.find({runtime:{$exists:true}}).pretty()`

* All movies where runtime  does not exists\
`db.movies.find({runtime:{$exists:false}}).pretty()`

* All movies where runtime exists and is greater than 42\
`db.movies.find({runtime:{$exists:true, $gt:42}}).pretty()`
> Note: if suppose there is a document with `runtime : null` and if you try `runtime:{$exists:true}` then it will retrieve the document even though it is null. if you want to fetch documnt where key exists and is not null then `runtime:{$exists:true, $ne : null}`

* All movies where runtime is a decimal\
`db.movies.find({runtime : {$type: "decimal"}}).pretty()`

* All movies where runtime is a decimal or a number\
`db.movies.find({runtime : {$type: ["decimal", "number"]}).pretty()`
> Note : You can either pass an array or a single value for `$type` operator

# Working with Evaluation Operators

In our movies collection, the `summary` key contains a string of description\
If we try searching for musical as summary using ` db.movies.find({summary: "musical"}).pretty()` then we get an empty resultset because it is looking for an exact summary as musical rather than looking for a word musical in the summary.\
Here is where `$regex` helps.\
`$regex` allows you to search for text or snaps of text\
` db.movies.find({summary: {$regex: /musical/}}).pretty()`
This looks for a word musical in the summary 

If you wanna compare two fields inside a document you can use $expr\
Suppose we have sales data 
```
{ "_id" : ObjectId("606db03ae99bd2878243e6ff"), "volume" : 100, "target" : 120 }
{ "_id" : ObjectId("606db084e99bd2878243e700"), "volume" : "89", "target" : "80" }
{ "_id" : ObjectId("606db0cbc9ce9c2c4055a66b"), "volume" : "200", "target" : "177" }
```
Now we want all documentwhere the volume is above the target\
` db.sales.find({$expr: {$gt: ["$volume", "$target"]}}).pretty()`
> Note: Here we should know the sequence of columns you want to compare. Also you should use $ before the column name to make mongoDB know that compare the value of the column.

Now we want all documents where if the volume is greater than or equal to 190 then subtract 30 from the volume and should be greater than target else if less than 190 then volume should be greater than target\
` db.sales.find({$expr: {$gt: [{$cond: {if: {$gte: ["$volume", 190]}, then: {$subtract: ["$volume", 30]}, else: "$volume"}}, "$target"]}}).pretty()`

```
{
        "_id" : ObjectId("606db084e99bd2878243e700"),
        "volume" : 89,
        "target" : 80
}
```
This is a very complex query and we will look at this in a better way in the aggregation module

# Working with Arrays
Suppose we have `flights` collection with each document having a `pilotDetails` as a array of embedded document and each embedded document has a `pilotName` and `age` fields\
Now if you want to find all the flights where the pilotName is "xyz"\
`db.flights.find("pilotDetails.name": "xyz").pretty()`

* All movies with 4 genres\
` db.movies.find({genres: {$size: 4}}).pretty()`
> Note: here in MongoDB you can only extract document which have an exact match when using `$size` 

* All movies where the genre is Action and thriller\
`db.movies.find({genres: {$all: ["action", "thriller"]}}).pretty()`
if we use\
`db.movies.find({genres: ["action", "thriller"]}).pretty()` it will look for document which follow the exact order of genres in the array and exclude the ones which dont.\
Wheres `$all` does not care for the order as long as they are present in the array
```
MongoDB Enterprise > db.movies.find({genres: {$all: ["Drama", "Thriller"]}}).count()
36
MongoDB Enterprise > db.movies.find({genres: ["Drama", "Thriller"]}).count()
4
```

suppose we have a users collection
```
{
        "_id" : ObjectId("606dbc4d77c054194a65d819"),
        "name" : "max",
        "hobbies" : [
                {
                        "type" : "Sports",
                        "frequency" : "2"
                },
                {
                        "type" : "Cooking",
                        "frequency" : "3"
                }
        ]
}
{
        "_id" : ObjectId("606dbd28ea77de2a0871c249"),
        "name" : "Hana",
        "hobbies" : [
                {
                        "type" : "Sports",
                        "frequency" : "3"
                },
                {
                        "type" : "Cooking",
                        "frequency" : "2"
                }
        ]
}
```
Now if we want users who have hobbies like sports and the frequency for sports is 3\
If we use this 
```
db.users.find({$and: [{"hobbies.type": "Sports"}, {"hobbies.frequency" : 3}]}).pretty()
```
we get `hana` and `max` but we were expecting only `hana`. This is because what `$and` does here is that it looks for documents where hobbies-type is sports and hobbies- frequency is 3 and they don't have to be the same document/embedded document.
so if you see in `max` one embedded document has type as sports and another document has frequency as 3 so it amends with the $and condition.

Here we can use `$elemMatch`\
` db.users.find({hobbies: {$elemMatch: {type: "Sports", frequency : 3}}}).pretty()`
What `$elemMatch` does is it tries to match the condition for each index of the array. Here being an embedded document so here it tries to match the condition for one embedded document as a whole and if that embedded document matches both the criteria then only it's included in the resultset else the search goes on for the next index in the array.


