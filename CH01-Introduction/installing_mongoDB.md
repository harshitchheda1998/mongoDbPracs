# Installing MongoDB Community version
* Download the latest MongoDB community server version
* All your MongoDB related commands/executables are there in the `/bin` folder of your MongoDB installed folder
* If you want you can store the path to the `/bin` folder as environment variables
* In Mac/Linux you should go to your `root` folder and there you should create a new folder called `data` and inside that create a new folder called `db`. You can also do the same in windows which can be any place unlike in mac/Linux where it needs to be in the root directory as the default location

# Starting mongoDb database server
> mongod --dbpath <path to your /data/db folder>

# Staring mongo client Shell
> mongo