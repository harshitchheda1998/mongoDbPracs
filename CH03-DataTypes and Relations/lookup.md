# Joining using `$lookup`
Let's look at a method to merge documents that are split due to the reference approach.\
Let's merge `authorDetails` and `bookDetails` collection\
**Author**
```
{
        "_id" : ObjectId("606bf48d703bf2c33523735e"),
        "name" : "Charles Darwin",
        "age" : 39,
        "address" : "NYC"
}
```
**Books**
```
{
        "_id" : ObjectId("606bf53a703bf2c33523735f"),
        "authorId" : ObjectId("606bf48d703bf2c33523735e"),
        "name" : "Fittest Survives",
        "totalCopies" : 8000
}
```

Now we can merge the documents by using the `aggregate()` method of aggregation framework along with `$lookup`
```
db.bookDetails.aggregate([{$lookup:{from: "authorDetails", localField:"authorId", foreignField : "_id", as:"creators"}}]).pretty()

{
        "_id" : ObjectId("606bf53a703bf2c33523735f"),
        "authorId" : ObjectId("606bf48d703bf2c33523735e"),
        "name" : "Fittest Survives",
        "totalCopies" : 8000,
        "creators" : [
                {
                        "_id" : ObjectId("606bf48d703bf2c33523735e"),
                        "name" : "Charles Darwin",
                        "age" : 39,
                        "address" : "NYC"
                }
        ]
}
```
* Here aggregate() takes an array of steps because we can have multiple steps before we need the final result.
* Each step is a document.
* For now, we are using only one step which is the `$lookup` and it takes a document as well.
* `$lookup` requires 4 fields
    * **from**: which target collection are you referring to for merging
    * **localField**: which field from your initial collection do you want to compare with
    * **foriegnField**: which field in your target collection do you want to compare with
    * **as**: alias name for the merged column

But this should not be the reason for using references because merging takes a hit on performance compared to embedding a document as an alternative.