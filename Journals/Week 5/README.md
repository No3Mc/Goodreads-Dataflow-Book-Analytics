# IMAT3104 Database Management and Programming Indexing

## Index and Explain with Zip Codes

Referring to the zip code collection that you have already loaded into MongoDB, do the following:

1. Write a query to count the number of zip code areas with a population of only one.

```db.zips.count({pop:1})```

2. Retrieve the entries counted in exercise 1 above, but add the explain method to the query with verbosity: “allPlansExecution”, like so:

```db.zips.explain("allPlansExecution").find({pop:1},{_id:true})```


A cursor to the explain document is returned instead of the document itself. In this case, use the following general JavaScript code to loop round the cursor to print the document.

``` var cursor = db.zips.explain("allPlansExecution").find({pop:1},{_id:true}) while (cursor.hasNext()) { print(cursor.next());} ```


The explain method returns information on the query plan for executing the find operation. That is, when a query is about to be executed a query optimiser is employed to work out the fastest way to process that query, taking into account the state of the database at that point in time. The state of the database includes factors such as the existence of indexes that allow fast access to documents in a collection. For further explanation: https://docs.mongodb.com/manual/reference/method/db.collection.explain/

Look at the execution stats and take note of the following results:
- `nReturned`: The number of results found. 10
- `totalDocsExamined`: The number of objects checked to do search. 29353
- `executionStages.stage`: Description of the operation. `COLLSCAN`, a full collection scan.
- `executionTimeMillis`: Total time in milliseconds required.

For further explanation: https://docs.mongodb.com/manual/reference/explain-results/

3. Now create an (ascending) index on the pop field.

```db.zips.createIndex({pop: 1})```

4. In MongoDB Compass, click on your zips collection and click on the Indexes tab to view the index created. Examine its details and the options available.

5. Repeat the query with the explain method from step 2. Look at the `totalDocsExamined` value now and compare it to the value before the index was created. Why is the value what it is? What details have changed?

6. Now create a compound index on the pop and _id fields.

```db.zips.createIndex({pop: 1, _id: 1})```

7. Repeat the query with the explain method from step 2. Which index is named in the plan? Look at the `totalDocsExamined` value now. Why is the value zero?

## Exercises

1. Search for entries with the “CA” state, using explain. Look at the execution plan. Make a note of the results.

2. Now create an index on the State field, search for “CA” and look at the new explanation. How do the results compare with those without an index?

3. Search for the city of Pasadena in CA and look at the Explain results.

4. Create a compound index on city and state and repeat exercise 3 above.

## Geospatial Indexes and queries

Create a 2d geospatial index on the location field. Read more about them here: https://docs.mongodb.com/manual/tutorial/build-a-2d-index/

### Exercises

Have a look at https://docs.mongodb.com/manual/tutorial/query-a-2d-index/ and then write queries to do the following:

1. Find all the cities within a box with (-100,-100) at one corner and (30,30) at the other. [Hint: use $box].

```db.collection.find({location: {$geoWithin: {$box: [[-100, -100], [30, 30]]}}})```

2. Find all the cities within approximately 20 miles of Mount Rushmore (-103.459067 longitude, 43.879102 latitude) (Note: 1 degree is approximately 70 miles). [Hint: use $center].

```db.collection.find({location: {$geoWithin: {$center: [[-103.459067, 43.879102], 20 / 69]}}})```


