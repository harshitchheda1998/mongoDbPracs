# Schema planning
Ask these questions before you start modeling your schema
* Which Data does my app need to generate
	* Eg: User Information, Product information
	* Defines the fields you need and how they relate
* Where do I need the data
	* Eg: Welcome page, Product list page
	* Defines the required collections and field grouping
* Which kind of data or information  do I need to display
	* Eg: On Home Page: Product Names with images
	* Defines which queries you'll need
* How often do I fetch my data
	* Eg: For every page reload
	* Defines whether you should optimize for easy fetching i.e is data duplication required 
* How often do I write or change my data
	* Eg: Often or rarely
	* Defines whether you should optimize for easy writing i.e is data duplication required since the change will have to be done at every end.

# Understanding relationship types
* Nested/Embedded Documents
* References

**Based on what the requirement is you can decide on what to go for whether to go for embedded documents to maintain a relationship or go for referencing**
Eg: For a customer and address, it is better to have nesting of addresses inside the customer because every customer has only one unique address. So there is no need for two collections and then using references between them.
On the other hand, imagine users and a list of favorite books. Here, we can use references between user and book collections since multiple users might be having the same taste of books, and updating a ceratin book is easy in this case.

# Scenarios for each relation with both approaches
* **One to One relation**
	* **Embedded Document**\
	Eg: The patient and his Summary. Here if we need the patient's details along with his disease summary then we can go for embedding style.

	* **Reference**\
	Eg: Customer and car[Not car model]{One person owns only one car}. Here if we need customer details for doing some analytics and there is no need for car details then we can go for references which will help to send fewer data over the wire. Thus this helps in reducing more of the transformation work that needs to be done if we use embedded document over referencing.

* **One to many relations**
	* **Embedded document**\
	Eg: Post and its comments. If you need to fetch all the post details along with its comments on a page.

	* **Reference**\
	Eg: City and citizens. Imagine you want to analyze city details along with the meta rather than the list of citizens because a city might have a huge population and bring each citizen detail along with city details will be overhead.\
	    If you store citizen details along with the city details then you might even reach or pass the document size limit which is 16MB/document.

* **Many to Many relations**
	* **reference**\
	Eg: Customers and products/ books and authors
 
> Note: The majority of the time for many to many relations it is preferred to use references.\
> Note: Based on application requirements like analytics/ page population one should decide whether to go of embedding or references\
> Note: Reference approach is best when dealing with shared data across collections.