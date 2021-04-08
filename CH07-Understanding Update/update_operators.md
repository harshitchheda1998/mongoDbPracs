# Field Update operators:

* `$set` operator\
It first checks for documents to be updates by the filter specified and then tries to add a field if it does not exist or tries to override the field if it exists even if the datatype of the key previously was different.\
`db.users.updateOne({name : "chris"}, {$set : {phone : "1234567890"}})`
> Note: `$set` works with `updateOne` and `updateMany`
> Note : You can also update multiple keys in a document using `$set` like `db.users.updateOne({name : "chris"}, {$set : {phone : "0987654321", age : 40}})`

* `$inc` operator\
`db.users.updateOne({name: "Manuel"}, {$inc: {age: 1}})`\
This will update the age of Manuel by 1\
You can also do multiple update operations in the same update query\
`db.users.updateOne({name: "Manuel"}, {$inc: {age: 1}, {$set: {isSporty: false}}})`\
If you pass negative value to `$inc` then it works as decrementing operation.\
`db.users.updateOne({name: "Manuel"}, {$inc: {age: -2}})`\
If you try doing multiple operations on the same field in the same update query then you might face a conflicting error from mongoDB\
`db.users.updateOne({name: "Manuel"}, {$inc: {age: 1}, {$set: {age : 30}}})`

* `$min` & `$max` operator\
`db.users.updateOne({name: "Chris"}, {$min: {age: 35}})`\
This will change the age of Chris only if the new value of age is higher then the previous value.
This means `$min` updates a field only if the new value of a field is lower than the previous value.

The same is the case with `$max`. It updates a field only if the new value of a field is higher than the previous value.\
`db.users.updateOne({name: "Chris"}, {$max: {age: 39}})`

* `$mul` operator\
`db.users.updateOne({name: "Chris"}, {$mul: {age: 1.1}})`\
This will increment the age of Chris by simply multiplying the previous age with 1.1

* `$unset` operator\
`db.users.updateMany({}, {$unset : {age : 15}})`\
This will simply get rid of the key from the document which matches the filter.\
Here the value that you specify in the field you want to unset can be anything like "" or null. What matters is that the field name is specified.
So the above query will also remove the same documents as this query\
`db.users.updateMany({}, {$unset : {age : ""}})` or `db.users.updateMany({}, {$unset : {age : null}})`

* `$rename` operator\
`db.users.updateMany({}, {$rename: {age: "totalAge"}})`
This will rename the field `age` to `totalAge`

# Working with `upsert`
`upsert` adds a document if it does not exist else it updates the document if it exists.\
`db.users.updateOne({name: "Maria"}, {$set: {age: 40, hobbies: [{type: "Foody", frequency: 3}]}}, {upsert: true})`\
This will look for the document with name as Maria and if it does not exist it will simply add a document with a field `name: "Maria"` even though we dont pass in the `$set` else it will update the document.
> Note: by default `upsert` is set to false for both updateOne and updateMany.


# Array Update operators
* `$` operator for updating matched array documents\
If we want to add/update a field inside hobbies array where the docuemnt inside the array follows both the criteria i.e the type is Sports and the frequency is 3 then\
If we do this\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {hobbies:[highFrequency : true]}})`\
this will change the hobbies key entirely and not just the sub-array document which was matched.\
We can do this by using `$` operator\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$" : {type: ..., frequency: ... , highFrequency: ...}}})`\
Here what does `$` operator does is that for the sub-array document which matched, it changes that sub-array document. But this is also not right since we only need to add/update a key nad not modify the sub-array document entirely.
So now the solution is\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$.highFrequency" : ...}})`\
Here we just simply appended the key we want to update/add to the `$` operator for the sub-array document which matched the filter.

So we saw the use of `$` operator with field appending using dot operator and also without the field appending.

WE can also set various other field other than the array field in the same update as well\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$.highFrequency" : ..., age : 40}})`

* `$[]` operator for updating all array documents\
Suppose in our hobbies array we  have three documents all of type Sports but one with frequency-3 and the other with frequency-3 and frequency-1.\
Now if we use the above query\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$[].highFrequency" : ...}})`\
You will observe that only one sub-array document got updated. Even though the document with frequency-3 also matched the criteria\

Here is where we can  use `$[]` operator\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$[].highFrequency" : ...}})`\
This will set highfrequency for all the sub-array document even if they dont match the criteria of search


* `$[]` with an identifier \
The above query with `$[]` also didn't match the result we wanted. It updated all the array documents which didn't match the query as well.

What we can do here is use an identifier for the array element we want to update and then check for the array element at that identifier and then update it.\
`db.users.updateMany({$elemMatch: {type: "Sports", frequency: 3}}, {$set : {"hobbies.$[el].goodFrequency" : true}}, {arrayFilters: [{"el.frequency": 3}]})`\
Here `el` is an identifier given by the user which is used to specify the array element you want to update. the identifier can have any name.\
Before updating we can check whether we need to update that element or not by using the array filter option which takes an array of filters you can use to filter out the array and this filter can be different from the filter we use as the first-option while filtering the document.
This queries all the array elements in the array which match the filter.

Now suppose we want to set the low frequency for hobbies-items as true for users whose age is below 30 and the frequency is less than 2.\
 `db.users.updateMany({age : {$lt: {30}}}, {$set : {"hobbies.$[el].lowFrequency" : true}}, {arrayFilters: [{"el.frequency": {$lt: {2}}}]})`\
If you see here the first filter for the document and the second filter for the array are different which is fine.


* Adding elements to array using `$push` operator\
`db.user.updateMany({name: "Maria"}, {$push: {hobbies: {type: "Hiking", frequency: 5}}})`
This pushes an item onto the hobbies array

Now suppose you want to push more than one item together.\
Here we can use `$each` with `$push`\
`db.user.updateMany({name: "Maria"}, {$push: {hobbies: {$each: [{item-1 docuemnt}, {item-2 docuemnt}]}}})`

If you want to sort the doucments before pushing them onto the array you can use `$sort` with `$each`\
`db.user.updateMany({name: "Maria"}, {$push: {hobbies: {$each: [{item-1 docuemnt}, {item-2 docuemnt}], {$sort: {frequency: -1}}}}})`\
And this will sort the documents before inserting them. A nice thing is that your entire array after insert will also get sorted when you use `$sort`\
So Imagine you had [1,5,3] in the array and you try to push [2,1] onto the array with `$sort` in ascending order then the final document to be stored in the DB will have array as [1,1,2,3,5].

You can also you `$slice` as well with `$each` if there are more number of items which have requested an isert into the array together but you want only the first n- number of items to be inserted\
`db.user.updateMany({name: "Maria"}, {$push: {hobbies: {$each: [{item-1 docuemnt}, {item-2 docuemnt}], {$slice: 2}}}})`

* removing elements from array using `$pull` operator\
with `$pull` you can specify which element you want to remove. At the condtion can be a simply equality or a complex comparioson for removal of that element/elements\
`db.user.updateMany({name: "Maria"}, {$pull: {hobbies: {type: "Hiking"}}})` or for some complex comparison like `db.user.updateMany({name: "Maria"}, {$push: {hobbies: {type: "Hiking", frequency: {$gt : 5}}}})`
So `$pull` removes all the elements from the array which match the criteria.

* removing the first/last element from the array using `$pop` operator\
`db.user.updateMany({name: "Maria"}, {$pop: {hobbies : 1}})`\
With pop you just speicigy from which array you want to reove the element and with the flag.\
The flag can be `1` for the last element or `-1` for the first element

* `addToSet` operator\
With `$push` you can add duplicate items into the array but addToSet tries to add unique value only. So if you try to add an item that is already present inside the array it cannot be added again.\
`db.user.updateMany({name: "Maria"}, {$addToSet: {hobbies: {type: "Hiking", frequency: 5}}})`\
You won't find any error if you try to insert a duplicate item but it won't modify the array.