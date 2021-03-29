## What is MongoDB?
* It is most importantly a database.
* The company behind MongoDB is also called MongoDB.
* The name MongoDB stems from the word humungous cause this database is built to store lots and lots of data not just from the data size perspective but also that u can store lots and lots of data and then u can work with it efficiently.
* ***So overall we can say it is a database solution.***

But we have got lots of database solutions already like SQL, Postgres and so on.\
__So how is MongoDB different?__

## How it works
So MongoDB is a database server that then allows you to runs various databases on it.\
Eg: We have a `shop` database running on our MongoDB server.\
In the MySQL world, we also have a database and in such a database we have tables.\
In MongoDB, we have so-called collections\
Eg: Inside our `shop` database we might have `users` and `orders` collections.\
We can have multiple databases and multiple collections inside each database.\
Now inside a collection, we have documents.\
Eg: Inside `users` we might have a document
	`{ name : 'Max', age : 29 }`
	`{ name : 'Manu' }`\
If you have noted that it looks like a javascript object and that's how we store data in MongoDB.
Also if you have noted inside a collection you are schemaless.\
`{ name : 'Manu' }` does not look like `{ name : 'Max', age : 29 }`\
This is the flexibility that MongoDB gives you whereas SQL-based databases are very strict on the data you want to store in there.\
In MongoDB, you can store different data in the same collection and your database can grow with your application as per your application grows or if the data requirements are not yet completed.
Of course, you will be having some form of structure in your collection because your app will be requiring some form of structure to work with the data.

Inside MongoDB, to store objects, we use a format called JSON
```
{ "name" : "Max",
  "age" : 29,
  "address" :
    {
      "city" : "Munich"
    },
  "hobbies" :
    [
      { "name" : "Cooking"},
      { "name" : "Sports"}
    ]
}
```
this entire object stating from `{` and ending with  `}` is called one document.

Here,
`"name" : "Max"` is called a key and `name` is called the name of the key which has to be surrounded in double-quotes always
and `Max` is called the key value. If the value is a string then it also has to be in double-quotes but you can also store numbers without quotes or booleans (true or false).
You can even store nested data or embedded document
```
{
      "city" : "Munich"
}
```
__So how is this embedding helpful?__\
So now you can create complex relationships between data and store them in the same document which makes working and fetching it super efficient whereas in SQL you need to write complex joins to get data from TableA and TableB.
So in MongoDB, you can store data efficiently and logically.
You can also store a list of an embedded document or a list of strings or numbers
```
 [
      { "name" : "Cooking"},
      { "name" : "Sports"}
    ]
```
or
 `[ "Cooking", "Sports" ]`
or
`[ 1,2,3 ]`
Behind the scenes on the server, MongoDB converts your JSON data to a binary version (BSON) which can be stored and queried efficiently

__So overall mongoDB is :__
* SchemaLess
* Has No/Few Relations
