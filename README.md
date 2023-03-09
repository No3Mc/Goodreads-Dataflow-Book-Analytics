Solutions:

[Week 1](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%201/Week%201.txt)
[Week 2](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%202/Week%202.txt)
[Week 3](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%203/Week%203.txt)
[Week 4](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%204/Week%204.txt)
[Week 5](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%205/Week%205.txt)


[Final](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md)

[3.2 Understanding the data.](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md?plain=1#L42)

[3.3 Getting your personalized data.](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md?plain=1#L142)

[4.0 CLEANING THE COLLECTIONS]()



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

| Host                   | Username | Password | Authentication Database |
| ---------------------- | -------- | -------- | ------------------------ |
| mongodb.dmu.ac.uk      | cwuser   | cwuser   | IMAT3104Coursework        |


If you are having difficulties with the credentials above, paste the below URI in the Connection String window

    mongodb://cwuser:cwuser@mongodb.dmu.ac.uk/?authMechanism=DEFAULT&authSource=IMAT3104Coursework

Note: At the moment, you can only connect to the MongoDB server while on university network. This means either you are on the University lab machines or you connect via Horizon. If you wish to work on your PC, you have to complete the ETL using above methods then move to your PC and import the personalized datasets.

3.2 Understanding the data.

Study all the collections (read more here: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home) and answer the following:

1. What is/are the unique/identifying key(s) of the documents of each collection? [4 marks]

2. Study the collections briefly with their fields and values. [6 marks]
 
  a. Identify issues and anomalies you have seen such as invalid data, empty fields, etc.?

  b. Identify examples and how you intend to address them. You do not need to solve the issues here.

Complete the table below based on 2(a) and 2(b).

Collection | Field | Anomaly | Examples | Solution Plan
--- | --- | --- | --- | ---
<b>Books.json:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_book_authors | ex | ex | ex | ex
<i>goodreads_book_genres_initial | ex | ex | ex | ex
<i>goodreads_book_series | ex | ex | ex | ex
<i>goodreads_book_works | ex | ex | ex | ex
<i>goodreads_books | ex | ex | ex | ex
<b>Genre.Children.json:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.Comics n Graphics:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.Fantasy n Paranormal:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.History n Biography:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.Mystery, Thriller n Crime:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.Poetry:</b> | <b>=========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
Genre.Romance:</b> | <b>=========</b> | <b>=========</b> | <b>=========</b> | <b>=========</b>
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
ex | ex | ex | ex | ex
<b>Genre.Young Adult:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
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
was only scrapped from the internet and incomplete or not in proper format. Remember, in Section 3.2, you identified some anomalies on the dataset. For each anomaly that happens to be in your personalized dataset:

1. Provide for a minimum of four (4) anomalies. [4 marks each]

 a. Take screenshots of sample documents with the anomaly before it is corrected.
 b. Show the query/queries used to address this anomaly.
 c. Take screenshots of samples of documents after the anomaly has been corrected.

2. Create a new field publication_date in the format YYYY-MM-DD that merges publication_day, publication_month, and publication_year [2 marks]

3. Create another field in the books collection unix_publication_date that converts
publication_date to Unix format. [2 marks]



5.0 QUERYING THE COLLECTIONS [20 marks]


Write the following queries (Q1 to Q4) against the collections that you personalised and loaded you’re
your local MongoDB. For each query, submit the MongoDB command in plain text, AND present it with its
results as a screenshot showing the command and the documents returned. For example:
//Q1001. Find the details of the product identified as " B001KTEBOG ".
db.p77342060_softwares.find({asin:"B001KTEBOG"})

![image](https://user-images.githubusercontent.com/41834061/221639935-5b74f24d-c3d2-48dc-a8f1-a7c49569350f.png)

If a lot of documents are returned, show the first set of documents that are displayed. Ensure that what is
displayed answers the requirements and demonstrates that the query is correct. You must do this to gain
all the marks available for a question.


Q1. Find top 3 books with highest average rating above 3.5 and are not e-books. Display only book id, title,
price, and average rating. [5 marks]

Q2. Who is the most famous reviewer in your dataset? Most famous reviewer have the highest number of
5-rated reviews in your datasets. Display the user_id, average rating, average n_votes, total
n_comments and total number of reviews written by this reviewer. [6 marks]

Q3. Update the most famous reviewer from Q2 by adding a new field named most_famous and set its
value to true. [4 mark]

Q4. Using reviews and books collections, find the title and price of 3 most expensive books reviewed by
the most famous reviewer. No other product details are required. [5 marks]



6.0 IMPLEMENT AND EXPLAIN INDEX FOR THE DATABASE [15 marks]

 1. Identify the chosen query from Section 5 and explain/justify your choice of index. Why do you think
 indexing could improve the query. [5 marks]

 2. Implement one index that would improve the querying of the database based on one of the queries
 (Q1-Q4). [2 marks]

 3. Present the execution plan of the selected query before the index and after the index has been
 created. Compare and discuss the execution plans to support your choice and summarise your
 findings. [8 marks]

7.0 RE-DESIGN THE DATABASE USING AGGREGATE DATA MODELLING [20 marks]

Write code in MongoDB to automatically embed the details of authors from authors collection and genres
from genres collection with their corresponding book in the books collection. This is an aggregate data
modelling (ADM) question.
Therefore, the books collection will contain the book data, authors data and genre data in a single
collection of books. This requires the following tasks:

 1. Make new copy of the pxxxxxx_books collection and name it pxxxxxx_books_adm. [1 mark]

 2. Embed all the authors and genres of books into their corresponding book using the new
 pxxxxxx_books_adm collection. [6 marks]

 3. To verify that the changes to the new products collection were successful, display the title,
 authors, and genre of the book with highest number of pages. [1 mark]

 4. Note that there is no longer need to reference the book_id inside each genre or author_id inside
 each book now that they are part of the book in pxxxxxx_books_adm collection. Show that these
 are removed. [2 marks]

 5. Using $lookup operator, fetch the complete information of 50% of books with their authors and
 genres using pxxxxxxx_books collection. Track the speed of the query. [3 marks]

 6. Using the pxxxxxx_books_adm collection, fetch the same information as above. Track the speed
 of the query. [3 marks]

 7. Compare the performance results of 7(5) & 7(6) above and write a brief discussion about the
 results. [4 marks]


All tasks must each be fully automated by writing code. Note that some tasks may take many seconds or
minutes to execute depending on the machine.

8.0 WEEKLY JOURNALS [10 marks]

Report all your journal submissions by completing the table below:

| Week No | Status/Reason                              |
| ------- | ------------------------------------------ |
| 1       | For example (Submitted on time)            |
| 2       | For example (Late submission due to xyz)   |
| 3       | For example (No submission due to abc)     |
| 4       |                                            |
| 5       |                                            |



9.0 DELIVERABLES

1. You are required to upload your answers to the questions into Turnitin on Blackboard as one
PDF file. Do NOT submit any JSON file.

2. Add your P number to your filename.

3. Your file must be in a readable format so the NoSQL code in plain text can be copied and executed
from it.

4. In Turnitin, press both the Upload button and the Confirm button to submit your file and receive
a receipt.

10.0 CLOSING COMMENTS

1. This is an individual assignment. Do not negotiate with others to clarify what is required. By
uploading your work to Turnitin in Blackboard you will be declaring that the work you submit is
your own and not plagiarised in any way.

2. Tutors are prepared to offer help with the part of the assignment where there is clear evidence
that you have made a substantial attempt but have become stuck.

3. Mark totals are shown alongside each question. Your tutor will need to able to execute your NoSQL
code in MongoSH to check for correctness. Marks will be awarded for correctness and
presentation.

4. Marks will typically be deducted if information is erroneous, missing, irrelevant or difficult to
ascertain. Marks can also be deducted if the answer is particularly inefficient. Therefore, partial
marks are available if an answer is partially correct or partially presented.

5. There is often more than one way to answer a question and so it is possible to gain full marks for
an answer even if it is different from the markers' specimen set of answers. However, if these
answers do not follow the examples and exercises taught on the module, they may be inferior in some way and so consequently marks may be deducted. Note that some questions hint at the most appropriate method to use.


REFERENCES
R. He, J. McAuley. "Modeling the visual evolution of fashion trends with one-class collaborative filtering".
WWW, 2016.
J. McAuley, C. Targett, J. Shi, "A. van den Hengel. Image-based recommendations on styles and substitutes".
SIGIR, 2015.
