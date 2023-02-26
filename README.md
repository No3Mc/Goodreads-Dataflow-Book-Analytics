## IMAT3104 DATABASE MANAGEMENT AND PROGRAMMING NOSQL
### MongoDB Coursework 2022-2023

**1.0 INTRODUCTION**
This is an individual assignment, which gives you an opportunity to demonstrate your knowledge of NoSQL and your ability to implement, query and design a MongoDB document database. You will be awarded marks for what is achieved. This assignment is worth 50% of the overall module mark.

**2.0 SCENARIO**
Goodreads is an American social cataloguing website and a subsidiary of Amazon that allows individuals to search its database of books, annotations, quotes, and reviews. Users can sign up and register books to generate library catalogues and reading lists. They can also create their own groups of book suggestions, surveys, polls, blogs, and discussions (copied directly from the Wikipedia page).
- Goodreads has a web page for each book e.g. https://www.goodreads.com/book/show/5333265
- or each review e.g., https://www.goodreads.com/review/show/2410025795
A subset of Goodreads data has been selected for this coursework. There is data on books, reviews, authors, and genre. Permission was granted to use the data for this coursework from https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home on condition that the authors’ research papers are referenced (Mengting W. and Julian J. M 2018), (Mengting W et al. 2019). It is not necessary to read these papers to complete this coursework.

**3.0 CONNECT, EXTRACT, TRANSFORM AND LOAD DATA (CETL) [15 marks]**
3.1 Connecting to DMU MongoDB server.
Connect to MongoDB server using MongoDB Compass with the following credentials. Please note that the username and password are in lowercase.

Host mongodb.dmu.ac.uk
Username cwuser
Password cwuser
Authentication Database IMAT3104Coursework

If you are having difficulties with the credentials above, paste the below URI in the Connection String window

mongodb://cwuser:cwuser@mongodb.dmu.ac.uk/?authMechanism=DEFAULT&authSource=IMAT3104Coursework

Note: At the moment, you can only connect to the MongoDB server while on university network. This means either you are on the University lab machines or you connect via Horizon. If you wish to work on your PC, you have to complete the ETL using above methods then move to your PC and import the personalized datasets.

3.2 Understanding the data.
Study all the collections (read more here: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home) and answer the following:
1. What is/are the unique/identifying key(s) of the documents of each collection? [4 marks]
2. Study the collections briefly with their fields and values. [6 marks]
a. Identify issues and anomalies you have seen such as invalid data, empty fields, etc.?
b. Identify examples and how you intend to address them. You do not need to solve the issues here.
3. Complete the table below.

Collection | Field | Anomaly | Examples | Solution Plan
--- | --- | --- | --- | ---
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex

 

3.3 Getting your personalized data. [5 marks]
You are assigned a specific dataset to work on the labs. This means, the results you will get from your queries will be different from other students. Check blackboard under My Grades -> CW Assigned Group.
1. Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. [2 mark]
2. Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews.
[1 mark]
3. Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors.
[1 mark]
4. Extract all matching genres of books in pxxxxxxx_books to a new collection named
pxxxxxxx_genres. [1 mark]




Export all your collections to your local machine. Then delete all newly created collections matching your
p-number only. Be careful not to delete other students’ collections. Disconnect from the DMU MongoDB
server and connect to localhost server. For those that would want to continue the assessment on their PC,
this is the time to move back to your PC!



4.0 CLEANING THE COLLECTIONS [20 marks]

4.1 Have a quick look at the collections on MongoDB Compass, you will realise that some of the data
was only scrapped from the internet and incomplete or not in proper format. Remember, in Section 3.2, you identified some anomalies on the dataset. For each anomaly that happens to be in your personalized
dataset:
1. Provide for a minimum of four (4) anomalies. [4 marks each]
a. Take screenshots of sample documents with the anomaly before it is corrected.
b. Show the query/queries used to address this anomaly.
c. Take screenshots of samples of documents after the anomaly has been corrected.
2. Create a new field publication_date in the format YYYY-MM-DD that merges publication_day,
publication_month, and publication_year [2 marks]
3. Create another field in the books collection unix_publication_date that converts
publication_date to Unix format. [2 marks]



5.0 QUERYING THE COLLECTIONS [20 marks]


Write the following queries (Q1 to Q4) against the collections that you personalised and loaded you’re
your local MongoDB. For each query, submit the MongoDB command in plain text, AND present it with its
results as a screenshot showing the command and the documents returned. For example:
//Q1001. Find the details of the product identified as " B001KTEBOG ".
db.p77342060_softwares.find({asin:"B001KTEBOG"})