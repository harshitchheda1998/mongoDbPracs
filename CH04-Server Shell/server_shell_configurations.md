* You can use the `--help` to check for various configurations for the MongoDB server

# Some important configurations for `mongod`
* Configuring the `--dbpath`\
`mongod --dbpath <path to /data/db folder>`

* Configuring the `--logpath`\
`mongod --logpath <path to log file>`

* Configuring the `--fork`\
`mongod --fork --logpath <path to log file>`
> Note: with `fork` it is necessary to use `--logpath`\
The use of fork is that it helps to run the MongoDB server as a background service rather than a foreground service. So it does not block the terminal and you can do your work on the same terminal.\
> Note: `--fork` works only on Mac/Linux and not on windows since on windows you can install it as a service itself
> Note: you can manually configure the MongoDB server as a windows service by using `--install`[navigate to /bin folder first]. You can also use `--dbpath` or `--logpath` along with it. Just keep in mind that you run the `cmd` as an administrator.

* Configuring using custom config.cfg file\
`mongod --config[-f] <path to config.cfg file>`
> Note: This is helpful because then later you can use this reusable blueprint config file for later installations as well. For more details visit: https://docs.mongodb.com/manual/reference/configuration-options/

# Ways to start mongodb server on windows
* `net start <mongoDb service name>`
or 
* *Run>services.msc>service_name>start*

# Way to stop mongodb server on windows
* `net stop <mongoDb service name>`
or 
* *Run>services.msc>service_name>start*
or
* *`use admin` > `db.shutdownServer()`*
> Note: this works for mac/linux as well
