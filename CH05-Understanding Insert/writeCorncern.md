# WriteConcern
We know that our shell or client code will interact with the MongoDB server which will then communicate with the storage engine to write in memory or files.\
Your data might store in the memory for data that is frequently queried and then get written in files

Now you can config the `writeConcern` for all the insert operations like `insertOne` or `insertMany` with an additional argument i.e the `writeConcern` argument which in turn is a document like
`{w:1, j: undefined}`.\
writeConcern simply is useful for greater security\
**w**: on how many instances(replicas if any) you need the server acknowledgment 
> Note: default is `w:1` which means we require the server to acknowledge the write operation which means that server is aware of the write operation.

**j**: stands for a journal. It is an additional file which the storage engine maintains like a TODO file. It can be helped to store operations which the storage engine needs to do which have not been completed yet like the write operation.
The storage engine is aware that it needs to store data on disk simply by write been acknowledge. The idea of the journal is that if the server happens to go down the file is still there and if the server restarts or recovers sit can look into the file and see what it needs to do since the memory might have been wiped out due to shutdown. So the write operation might be lost if it is not written to the journal and not written to the real data files.
> Note: the default is `j: undefined` or `j: false` i.e it is not being used.

**wtimeout**: How much time the server should take to report success for the write or cancel it. Be careful on what you keep as the timeout because if you keep it too small then all correct write might not work even though they are written but because the server is taking much time the operation failed.
> Note: if your journal is enabled it might take even more time because the storage engine adds an entry to the journal as well after writing it to file after a successful write.

Now, why do we need to write to the journal and not the database files instead?
The reason being that writing to the files is a performance-heavy job. whereas writing it to a journal is faster compared to files since the journal is a simple entry that includes both the operation and its associated index modifications.
For more details regarding the journal visit: https://docs.mongodb.com/manual/core/journaling/

`{w: 1, j: true}` means that report only those write operations that have been acknowledged and have been written to the journal.
`{w: 1, j: true, wtimeout: 200}`  means should get completed within the timeframe

# The basic flow of the insert operation if journal is enabled:
 clinet -> server -> storage engine -> journal -> files

```
db.personData.insertOne({name:"max", age:29}, {writeConcern:{w:0}})

{ "acknowledged" : false }
```
Since the server is unaware to send an acknowledgment and you don't wait for the server to register the entry. So the storage engine didn't have time to store the data and generate an objectId. But the data gets inserted into the file. Because you sent a request but you don't even know whether it reached the server
And it is super fast.
There might be a case where you are simply logging the entries in the log and you don't want the acknowledgment for that even if the entry failed since its just a log so that  you can use `w:0`


# Atomcity
MongoDB helps in maintaining atomicity at a document level (including the embedded documents) i.e the document as a whole should get inserted and if any error occurs while inserting it then the document should be rolled back.


