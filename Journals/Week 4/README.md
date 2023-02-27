**Lab 04**

**More on Aggregation pipelines**

**Introduction**

This week’s lab will introduce how to connect to a server and fetch specific
dataset as required. We will then run some exciting queries on that datasets and present the results.

**Connecting to the server**

At the moment, you can only connect to the MongoDB server while on University
network. This means either you are on the University lab machines connected via WiFi/LAN, or you connect via Horizon. If you want to work on your PC, you may wish to complete exercise 1 & 2 on University machines, and then export the collections to your PC.

1. Start MongoDB Database

2. Start MongoDB Compass

3. Click on Advanced Connection Options

4. Under the General tab, change the Host to mongodb.dmu.ac.uk

5. Under the Authentication tab, select Username/Password

    a. Set the username to labuser

    b. Set the password to labuser

    c. Set the Authentication Database to IMAT3104Labs


6. Click on Connect

7. Upon successful connection, you should see the IMAT3104Labs Database
with products and reviews collections

**Getting your data**

Study both collections briefly with their fields and values. Identify issues you have seen such as invalid data, empty fields, e.t.c. Please make note of these issues in your journal.

You are assigned a specific dataset to work on the labs. This means, the results you will get from your queries will be different from one student to another.

Check blackboard under My Grades -> Lab Assigned Group

You will see a list that matches the group assigned to you. For example, a student with List [A, B, C, D] means they are assigned to documents in the products collection where the assignedGroup field has values A or B or C or D


Note: Remove duplicates if your list contains such. If your Lab Assigned Group is (A, C, C, D, D, F ), we remove duplicates and it is now (A, C, D, F)

Here is a sample document from the products collection


Here is a sample document from the reviews collection


Here is description of some fields from the collections

| Field | Description |
| --- | --- |
| asin | ID of the product, e.g. B00005BHKS (this is a real product on Amazon, click on B00005BHKS to view) |
| category | List of categories the product belongs to |
| description | Description of the product |
| main_cat | Main category of product |
| price | Price in US dollars (at time of crawl) |
| imageURL | URL of the product image |
| reviewerID | ID of the reviewer, e.g. A1RVTBD2QZ8TQ8 |
| reviewerName | Name of the reviewer |
| overall | Rating of the product |
| unixReviewTime | Time of the review (unix time in seconds) |


**Exercise 1**

1. Using aggregation pipeline, extract your assigned products data.
a. Hint (using $match and $in operators).
db.products.aggregate([
{$match: {assignedGroup: {$in:["A", "B", "C"]}}}
])
1. Write out your assigned data to a new collection to a new collection named
pXXXXXXX_products
a. Hint (using $out operator)
db.products.aggregate([
{$match: {assignedGroup: {$in:["A", "B", "C"]}}},
{$out: "pxxxxxx_products"}
])
1. Refresh the connection to see the new collection
2. Export this to your local computer
a. Ensure that you have selected your new collection
b. Go to the menu bar
c. Click on collections
d. Click on export collection
e. Select Export Full Collection
1. A JSON file will be downloaded
Exercise 2
1. Extract your set of reviews that matches the products in your
pXXXXXXX_products collection. There are some hints on how to do this
a. Get all products IDs from pXXXXXXX_products collection
var myasins = db.pxxxxxx_products.distinct("asin")
b. Get all reviews that are written for those products
var myasins = db.pxxxxxx_products.distinct("asin")
db.reviews.aggregate([
{$match: {asin: {$in: myasins}}},
{$out: "pxxxxxx_reviews"}
])
1. Export this to your local computer
a. Ensure that you have selected your new collection
b. Go to the menu bar
c. Click on collections
d. Click on export collection
e. Select Export Full Collection
1. A JSON file will be downloaded (takes a moment depending on the size of
your collection)
At this point, if you intend to stop using University machines and move back to
your PC, copy the two downloaded JSON files to your PC and continue Exercise 3
Exercise 3
1. Delete the newly created collections with your pnumbers only. Avoid
deleting other student’s collection or products and reviews collections.
1. Disconnect from LabUser connection
2. Connect to MongoDB localhost
3. Create a DB named AmazonProducts. Connect to this DB.
4. Import both JSON files exported from Exercise 2
a. Pxxxxxxx_products.json into Pxxxxxxx_products collection
b. Pxxxxxxx_reviews.json into Pxxxxxxx_reviews collection
Exercise 4
These exercises are to be completed on your extracted and imported local
database AmazonProducts
1. Using the most efficient approach, complete the table below. Remove the
Sample result column before submitting your Journal.
Item Sample result Actual result
Distinct
assingedGroups
[A, B, C, D]
Total Number
of products
8451
Total Number
of reviews
45122
Total Number
of products per
assignedGroup
[
{"_id": "A", "count": 3545},
{"_id": "B", "count": 5545},
{"_id": "C", "count": 7845},
{"_id": "D", "count": 1345},
]
1. The price field in the products collection needs some transformation
a. Remove the dollar sign from all prices (Hint: use $substr operator)
b. Convert the price field to double (Hint: use $toDouble operator)
1. Find the most expensive product using aggregation pipeline and simple
find(). Show both solutions.
1. Find the most reviewed product in your collection.
2. Find the reviewer with highest number of reviews.
3. Get the reviewerID, reviewerNames and number of verified reviews of the
top 10 reviewers ordered by number of reviews in desceding order.