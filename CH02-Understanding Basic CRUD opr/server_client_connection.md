# MongoDB Server and Shell connection
* Start mongoDB server on a different port along with your path to `data/db` folder\
`mongod --dbpath <path to data/db folder> --port <port_number>`

* Default port for MongoDB server is `27017`

> Note: If you change the port for your MongoDB server make sure when you try to connect the MongoDB shell you specify the updated port or else it will not establish a connection with the server because by default the MongoDB shell try to connect to the port `27017`
* Connect MongoDB shell on different port\
`mongo --port <port_number>`