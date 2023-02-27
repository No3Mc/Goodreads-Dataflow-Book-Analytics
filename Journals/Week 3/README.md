**IMAT3104 Database Management and Programming | Aggregation Methods**

**IMAT3401 Database Management and Programming**

**Zip Codes and Aggregation Methods**


**Operational details**

It is a good idea to keep a copy of your solutions in a text file so you can
reuse them for future exercises, coursework and revision.

**Data Download**

This lab work will use the zip codes collection from MongoDB. Download the
zips.json file from:

https://docs.mongodb.com/manual/tutorial/aggregation-zip-code-data-set/

**The document model**

View the contents of the zips.json file in a text editor. It contains tens of
thousands of individual zip code documents. Each document has the following structure:

    {
    "_id": "10280",
    "city": "NEW YORK",
    "loc": [
    -74.016323,
    40.710537
    ]
    "pop": 5574,
    "state": "NY",
    }

• The _id field holds the zip code as a string.

• The city field holds the city name. A city can have more than one zip
code associated with it as different sections of the city can each have a
different zip code.
• The loc field holds the location as a longitude latitude pair.
• The pop field holds the population.
• The state field holds the two letter state abbreviation.

Notice that the field names are all in lower case and the contents are all in
upper case.

**Import Documents**

Import the zip code documents into a new collection. If necessary, refer back
to the lecture notes. Remember this is a json file which is the default format.

**Query the Collection using Aggregate Pipelines**

Try out the following examples before attempting the exercises at the end of
this lab sheet.

**Example 1: Find all the states with a total population above ten million**

First let’s find the total population of each state:


The $group stage operator creates a document for each state, with id equal to the value of state. For example:

    { "_id" : "MN", "totalPop" : 4372982}

The loosely equivalent SQL statement for this aggregation pipeline would be:

    SELECT state, SUM(pop) AS totalPop
    FROM zips
    GROUP BY state;

Here the populations of zip codes in the same state are added together.

Next the results must be restricted to just those with a total population greater than ten million by adding the $match aggregation pipeline stage.


The $match stage operator filters the documents like a find(). For example:

{ "_id" : "CA", "totalPop" : 29754890}

The loosely equivalent SQL statement for this aggregation pipeline would be:

    SELECT state, SUM(pop) AS totalPop
    FROM zips
    GROUP BY state
    HAVING totalPop > (10*1000*1000);


Where the $match stage in this case functions in a similar way to the HAVING
clause in SQL.


**Example 2: Find the average size of city by population for each state.**

By way of example, there are two zip codes for the city of Aiken in the state of South Carolina. We need to add the populations of each zip code to calculate the total population of the city. Only then can we calculate the average size of a city in South Carolina.

    { "_id" : "29801",
    "city" : "AIKEN",
    "loc" : [-81.719429, 33.553024],
    "pop" : 51233,
    "state" : "SC" }
    { "_id" : "29803",
    "city" : "AIKEN",
    "loc" : [-81.594702, 33.531868],
    "pop" : 743,
    "state" : "SC" }

First, let’s use a $group stage to create new documents for every combination of city and state to calculate the sum of the populations for a city. We need to include the state because some city names appear in more than one state.



For Aiken this outputs the following document:

    { "_id" : {
    "state" : "SC",
    "city" : "AIKEN"
    }, "totalPop" : 51976 }

Check that the total population is correct.


Finally, let’s use a second $group to take these new documents from the first
$group as input. This second $group needs to group by state to calculate the
state’s average city population:



It should output documents like this:

    { "_id" : "SC", "avgCityPop" : 11139.6261980831 }

This is a powerful multi-group aggregation that is difficult to achieve in SQL.

**Example 3: Find the minimum and maximum longitude and latitude for all zip codes**

Let’s investigate a variety of aggregate pipeline stages. First, let’s replace the location array for each zip code with separate longitude and latitude fields, and then calculate the minimum and maximum longitude and latitude for all zip codes.

First let’s dismantle the location array so that its longitude and latitude
elements are each placed into their own separate document. To deconstruct
an array we will use the $unwind aggregation pipeline operator:


This outputs pairs of documents for each zip code. One document contains
the value from the first location index of the array (index 0) and the other
associated document holds the value for the second index (index 1). For
example:

    { "_id" : "01007",
    "city" : "BELCHERTOWN",
    "loc" : -72.410953,
    "pop" : 10579,
    "state" : "MA",
    "locIndex" : NumberLong(0) }
    { "_id" : "01007",
    "city" : "BELCHERTOWN",
    "loc" : 42.275103,
    "pop" : 10579,
    "state" : "MA",
    "locIndex" : NumberLong(1) }

Note that the includeArrayIndex field in the $unwind stage creates a
new field to hold the array index of the element, locIndex.
includeArrayIndex is optional but is very useful in this case because we
know that array index 0 holds the longitude value and index 1 holds the
latitude value.
Next let’s reshape each document so each loc field is named longitude or
latitude depending on the locIndex. To do this, we need a $project
stage:




The $project stage keeps the id, city, state and pop fields from the previous
stage but adds new fields for longitude and latitude. The longitude field is only
added if the locIndex is 0 and similarly the latitude field is only added if the
locIndex is 1. Each document at this stage only contains one of these fields
because they only have one element from the original array. For example:
{ "_id" : "01007",
"city" : "BELCHERTOWN",
"pop" : 10579,
"state" : "MA",
"longitude" : -72.410953 }
{ "_id" : "01007",
"city" : "BELCHERTOWN",
"pop" : 10579,
"state" : "MA",
"latitude" : 42.275103 }
IMAT3104 Database Management and Programming Aggregation Methods
Next let’s merge the pairs of documents together so that each zip code has
both a longitude and latitude field in the same document. For this we will
group the documents according to their zip code:
The $group stage creates one document for each zip code containing the city,
state and pop from the first document it finds. It does not matter which
document is found first because both documents for the same zip code
contain identical values for city, state and pop. The latitude and longitude
fields are added together because the value is null from the document where
they are absent which could be the first or last document. A pair of documents
should now be grouped into one. For example:
{ "_id" : "01007",
"city" : "BELCHERTOWN",
"state" : "MA",
"pop" : 10579,
"latitude" : 42.275103,
"longitude" : -72.410953 }
IMAT3104 Database Management and Programming Aggregation Methods
We could now finish by applying the $out stage to output the results to a new
collection, but instead let’s use the new documents to calculate the minimum
and maximum longitude and latitude for all zip codes:
Notice that this last aggregation pipeline stage specifies a group _id of null, to
perform the calculations on all documents in the collection. The results are:
{ "_id" : null,
"min_latitude" : 19.066844,
"min_longitude" : -171.701685,
"max_latitude" : 71.234637,
"max_longitude" : -67.00739 }
These range of latitudes and longitudes cover almost all of the USA.
For further information about different stages of an aggregate pipeline read:
https://docs.mongodb.com/manual/reference/operator/aggregation-
pipeline/#aggregation-pipeline-operator-reference
IMAT3104 Database Management and Programming Aggregation Methods
Exercises
Write the following MongoDB queries against the zip codes collection. You will
need to use the count, distinct or aggregate methods to answer them.
1. What is the total population of the city of CHICOPEE? There is more than
one zip code for CHICOPEE.
1. What is the total population of the state of SC?
2. How many different zip codes are there in the state of SC?
3. List all of the distinct states.
4. List all the cities in the state “NJ”. Create two solutions: one using
distinct and a second using aggregate.
1. List all the cities with a population between one and nine inclusive and
their population. Note that there may be cities with the same name in
different states.
1. List all the cities in the state “NJ” with a population of less than fifty.
2. What is the size of the largest city by population in each state?
3. List the distinct states that contain a city with a population of more than
three hundred and thirty thousand.
1.  What is the name of the largest city by population in each state? Hint: use
the $first or $last operator with the $group and $sort pipeline stages.
1.  List all the states and the number of cities they contain. Use an
aggregation pipeline, and note that {$sum:1} can be used to count the
number of documents in a group.
1.  List all the states that contain fewer than two hundred cities – extend your
pipeline from above.
1.  How many cities does state TX have? Create two solutions: one using
distinct and a second using aggregate.
1.  Which cities within the same state appear more than forty five times in the
collection (i.e. they have more than forty-five zip codes)?
1.  List all the city names that appear in more than twenty different states,
along with their states. (To list the states to which the cities belong, you
need to maintain a set of state names for each city. Use $addToSet for
this. The number of states is simply the size of that set.)