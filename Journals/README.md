LAB 01 - INTRODUCTION TO MONGODB


OPERATIONAL DETAILS


Try out all examples and exercises. It is a good idea to keep a copy of your solutions in a text file using a text
editor so you can reuse them for future exercises and revision.


STARTING MONGODB COMPASS



Launch MongoDB Compass from the start menu and click on CONNECT.


Click on MongoSH Beta, the MongoDB inbuilt command shell

CREATE A DATABASE

The default database is called test, but we will create a new one of our own. Type the following into the

MongoSH Shell:

MongoDB will switch to a database called mod (short for movies on demand). The command will create a new
database if it does not exist, otherwise it will return the existing database.


CREATE A COLLECTION


Create a new collection by typing the following into the MongoDB Client Shell:


The command is case sensitive.

INSERT DOCUMENTS

Populate the movies collection with the following movie using a ‘YYYY-MM- DD’ date format.


The insert will create a MongoDB document which is also known as a JSON object.


We can see a deprecation warning saying the insert() method is discontinued, but still supported. For the exercises, use insertOne() to insert one record, or insertMany() to insert multiple records

VIEWING DOCUMENTS IN COLLECTION

On the MongoSH, enter the following


Not pretty but you can see what you entered. We can use the MongoDB Compass GUI (most preferred)

a. Click on the refresh icon

b. Select the mod database

c. Select the movies Collection

d. view the same movie documents in its different modes.

e. Which view mode is the best for you?

EXERCISE 1
1. Insert the following movies into the collection using the following details:

- title = The Martian, directors = Scott, releaseDate = 2 October 2015, runtime = 144
- title = Amazing Spider-Man, directors = Marc Webb, releaseDate = 15-06-2012, runtime = 136
- title = Spider-Man: No way home, directors = Jon Watts, releaseDate = 23-Dec-2021, runtime = 148

2. Query the Collection.
Let’s be selective in what we find by restricting our search to just “zootropolis”:

Notice that the selector {title:"zootropolis"} is itself a JSON object

Warning! MongoDB is case sensitive. Try searching with tile : “Zootropolis”:


Nothing was found matching "Zootropolis" so MongoDB returns an empty result. If the field is the wrong case then MongoDB also returns an empty result, not an error message!

3. If your spelling is not great then you can just search for "zoo" using a regular expression: