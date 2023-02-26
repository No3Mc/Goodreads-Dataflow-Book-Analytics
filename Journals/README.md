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

Try this to ignore upper/lower case:

MongoDB documents more examples of regular expressions:


https://docs.mongodb.com/manual/reference/operator/query/regex/


EXERCISE 2

1. Find the movie called The Martian.

2. Find all movies with "tr" (or "Tr" etc.) in their title.

3. Instead of returning all the data in a document, let’s choose to display just the titles of the movies:

This second optional parameter (in the example, {title:1}) is called a projection. The first parameter is the
selector which we have already used that restricts the documents required.

In the example, the selection is empty ({}) so all documents are matched. This is needed because the first parameter in a find() is always the selector.


By default, the _id field is always returned.
However, it can be excluded:



Or if you prefer:

So you can include or exclude one or more fields explicitly, but inclusion and exclusion cannot be mixed except for _id.

EXERCISE 3

1. List only the title and runtime of all movies.

2. List only the director of the movie called The Martian.

3. It is possible to order the results by chaining a sort method to the find command:


The parameter for sort is a JSON object ({runtime:1}) where 1 is for ascending order and -1 is for descending order. So in the example the movies are sorted in order of runtime (the shortest first).

4. Another modifier called limit() can be chained with sort() to return the first set of documents:

So this finds the movie with shortest runtime (notice the time is in ascending order and limited to a single result).

EXERCISE 4

1. List and order the movies by title from A to Z.

2. Retrieve only the movie with the longest runtime.

3. Let’s find those movies with a runtime of greater or equal to 130 minutes:

Notice that the comparison operator (greater or equal to) is represented as $gte. All operators in MongoDB
are words prefixed with $.

Also notice that the comparison operator is inside a JSON object, {$gte:130}, which is itself embedded inside the selector which is also a JSON object, 

        {runtime: {$gte:130}}.
See https://docs.mongodb.com/manual/reference/operator/query/#query-selectors for a full list of
comparison operators.
4. MongoDB supports the use of arrays. The $in operator is used for matching one of several values in an array,
for example:
This returns any movie that is directed by howard or bush.
To match a single director, the $in operator is not required: