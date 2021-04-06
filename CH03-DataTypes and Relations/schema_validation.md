We know that MongoDB offers us the flexibility of loose schema. But sometimes you might want to lock the flexibility for the betterment of your application. Hence, Schema validation is helpful.

# Schema Validation
There are two operations with schema validation
* **Validate Level**
	* Which documents you want to validate whether insert ones or updates one
		* **strict**: All inserts/updates are checked
		* **moderate**: All inserts are checked, only previously correct documents are checked while an update

* **Validate Action**
	* What action to perform if validation fails
		* **error**: throw an error  and den  insert/update
		* **warn**: log the warning but proceed with the insert/update

# Ways to add validation
* During collection creation i.e creating collection explicitly
```
db.createCollection('posts', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  }
});
```
So you can insert validation using `createCollection` command and it takes the first argument as the name of the collection and the second argument is a document having a `validator` which is a document as well.\
The `validator` takes a document with `$jsonSchema` operator for the document schema.
* The keys for the schema:
	* **bsonType**: the datatype of the document/ key
	* **required**: which key are required to be present while insert/update operation
	* **properties**: the detailed schema for each key inside the document
	* **description**: as the name suggests just for the description of the key/document
	In the case of an array, we have one more key 
	* **items**: For the detailed schema for each item in the array

Now if you try to insert a document that does not follow the above schema then we get an error since the  default `validationAction` is **error**

If you want to change the action before the creation of collection then add a key `validationAction` after the validator key like this `validationAction: "warn"` or `validationAction: "error"`:
```
db.createCollection('posts', {
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  },
  validationAction: "warn"
});
```
If you already have a collection and you want to modify the collection schema use `runCommand` command which takes the collection name you want to modify using `collMod` key and the `validator` and the `validationAction` as the next set of keys if you want
```
db.runCommand({
  collMod: 'posts',
  validator: {
    $jsonSchema: {
      bsonType: 'object',
      required: ['title', 'text', 'creator', 'comments'],
      properties: {
        title: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        text: {
          bsonType: 'string',
          description: 'must be a string and is required'
        },
        creator: {
          bsonType: 'objectId',
          description: 'must be an objectid and is required'
        },
        comments: {
          bsonType: 'array',
          description: 'must be an array and is required',
          items: {
            bsonType: 'object',
            required: ['text', 'author'],
            properties: {
              text: {
                bsonType: 'string',
                description: 'must be a string and is required'
              },
              author: {
                bsonType: 'objectId',
                description: 'must be an objectid and is required'
              }
            }
          }
        }
      }
    }
  },
  validationAction: 'warn'
});
``` 
