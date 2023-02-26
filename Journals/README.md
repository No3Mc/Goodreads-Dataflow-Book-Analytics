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
See 

https://docs.mongodb.com/manual/reference/operator/query/#query-selectors 

for a full list of comparison operators.

4. MongoDB supports the use of arrays. The $in operator is used for matching one of several values in an array,
for example:


This returns any movie that is directed by howard or bush.
To match a single director, the $in operator is not required:

EXERCISE 5

1. List all movies with a runtime of less than 2 hours.

2. List all the movies directed by someone called "More".

3. Update the Collection. Change the release date of Zootropolis to 25th March 2016



Then write an appropriate query to verify that the change was successful.

So far, we have stored one release date per movie, but a movie typically has many release dates worldwide.

For example, the movie industry typically releases a Hollywood movie in the USA before Japan. That is, a
release date not only depends on the movie itself but also the country that is to show it.

Therefore, a country field should accompany the date. To do this we will make the release-date entry a JSON
object containing the date and country.

Each release date could also be stored as unix time which is the number of seconds that have elapsed since 1 January 1970. Unix time can be used to sort the movies into chronological order of release.

For example, update The Martian movie so its release date is for the USA and represented in Unix time.



EXERCISE 6

1. Update Zootropolis so its release date is for the UK and is also represented in unix time.

2. The new releaseDate field consists of a document (or object) inside the movie document. The releaseDate is said to be a child object of movie and the movie is the parent of the releaseDate. To query child objects we use dot notation and double quotes. 

3. For example, to find all the movies with a USA release date, write the
following:

EXERCISE 7

1. List and order the movies in chronological order of release
(hint: sort on unix time)

1. Update the The Martian movie so that it has one UK release date and one USA release date. The release date will become an array where each element is the release date for a particular country.

First let’s create an array with the USA release:



Notice the use of square brackets for the array. Next add the UK country release to the array:

Notice the use of the array operator: $addToSet to add the new release date to the release date array.
Check that the updates worked.

See

https://docs.mongodb.com/manual/reference/operator/update-array/ 

for more update operators.

EXERCISE 8

1. Update the Zootropolis movie so that it has one UK release date and one USA release date (4 March 2016).

2. It is important to be able to find the data we want from deep inside a document. Say we want to find the UK release date of the Martian movie. We try something like this:



However this shows the release dates for all the countries. To retrieve just the UK release date of the Martian movie the positional $ operator is included:

https://docs.mongodb.com/manual/reference/operator/projection/positional/

EXERCISE 9

1. Write a query to return only the USA release date of the Zootropolis movie.
We can easily add a new field to a movie by updating it:

So a new genre array has been added to The Martian movie. It is possible to find all movies with a genre.


Notice that the other movie, Zootropolis, does not have a genre field so it is not in the result. This is special to NoSQL where records of the same type can have different fields.

**Delete a document**

First add another movie with the following details:

    Title = Jurassic Shark
    Director = O'Donnell
    releaseDate = 5 November 2000
    runtime=60
Now delete this movie.



Notice that the JSON part of the query is the same as a find command.


**Insert another document**

Add another movie called ‘Good Will Hunting’ by ‘Van Sant’ released in the USA on 9th January 1997 and in
the UK on 6th March 1998 with a runtime of 126 minutes.

Query the movie to make sure you entered it correctly.

**Create another collection**


Add a new collection for actors to record who plays in which movies. To do this, run the following
commands:

1. Create a collection called actors.

2. Insert a document for an actor with name=’Matt Damon’.

3. Record the fact that Matt Damon has played in the movies “The Martian” and “Good Will Hunting” using
an array entry in his document.

4. Query the document to make sure that you entered it correctly.


EXERCISE 10

1. Find everyone who played in ‘Good Will Hunting’.

2. Find everyone who played in ‘Good Will Hunting’ and ‘The Martian’.

3. Find everyone who played in ‘Good Will Hunting’ or ‘The Martian’. [hint: use the $or operator].

**Finding information based on more than one collection**

If we want to find the directors of the movies that Matt Damon played in then we will need to search both
collections. Matt Damon is in the actor collection and the directors are in the movie collection. First we need to find all the movies that the actor Matt Damon played in from the actor collection and then find the directors that directed those movies from the movie collection. This requires two steps:


The opposite of $in is $nin, which is not in.

**JavaScript**

The MongoDB shells run JavaScript. If you execute a command and omit the parentheses (), you will see the
body of the JavaScript rather than executing the command:



Use JavaScript to print out the movies:

Reference

The MongoDB Manual: https://docs.mongodb.com/manual/