# Importing the data file to the collection
If you are in shell previously, exit the shell
`mongoimport -d <database name> -c <collection name> <path to the the JSON file you want to import>`

# Other option you can use with `mongoimport`
* `--jsonArray`: if your JSON file to be imported contains an array of documents to be imported then you should mandatorily use this option as well because `mongoimport` is unaware than to import the entire array of documents from the file. It is only aware of a single JSON document.
* `--drop`: if you want to drop the collection if it exists previously before you insert the data from the imported file you can use the option. 

# Import tv-shows data file in collection
`mongoimport -d moviesData -c movies tv-shows.json --jsonArray --drop`