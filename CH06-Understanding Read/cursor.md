# Cursor Object

When you run a `find()` on the server-side the query gets executed and it loads all the documents in server memory. But it sends only the first specified number of documents back in a cursor object.

# Some of the commands you can use on cursorObject
* `count()`: returns a count of all the documents, not just the documents returned in the cursor but also the documents remaining to be fetched.
```
var cursor = db.movies.find()
cursor.count()
240

or
db.movies.find().count()
240
```

* `next()`: returns the document from the position where the cursor is pointing. If the cursor has fetched all the documents already residing on the cursor then it fetches the next document from the server.
```
var cursor = db.movies.find()
cursor.next()
```

* `hasNext()`: returns a boolean whether more documents are there on the server to be fetched
```
var cursor = db.movies.find()
cursor.hasNext()
```

* `forEach()`: exhaust the entire cursor as well as all the documents which the cursor is yet to be fetched and brings them to the client one at a time
```
var cursor = db.movies.find()
cursor.forEach(doc => {printjson(doc)})

or
db.movies.find().forEach(doc => {printjson(doc)})
```

* Tip
If suppose we use \
`var cursor = db.movies.find()`\
and then we use \
`cursor.pretty()`\
Then it fetches all the documents residing on the cursor.\
Now if we try `cursor.next()` Then it will fetch the next document which is not there in the cursor.\
Same for `cursor.pretty()` if we again use this command then it fetches the next batch from the server.

# Sorting the cursor results using `sort({key : order [, key : order]})`
* Sort movvies based on average rating in descending order
`db.movies.find().sort({"rating.average": -1}).pretty()`
or
```
var cursor = db.movies.find()
cursor.sort({"rating.average": -1}).pretty()
```
> Note: 1-> ascending, -1 -> descending

* Sort movies based on average rating in descending order first and then based on runtime in ascending order
` db.movies.find().sort({"rating.average": -1, runtime : 1}).pretty()`

# Skipping or Offset the cursor results using `skip()`
```
db.movies.find().skip(10).pretty()
```
This query skips the first 10 documents from the cursor
> Note : `skip()` and `skip(0)` work the same way. They don't skip any documents


# Limiting cursor result using `limit()`
```
db.movies.find().limit(10).pretty()
```
This query limits only 10 documents to be fetched from the cursor.
> Note: `limit()`  means bring all documents from the cursor
> Note: if you now try using cursor.hasNext() it will return false because the server queried only the 10 documents and stored them in the cursor thus helping to boost the performance.