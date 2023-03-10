Solutions:

[Week 1](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%201/Week%201.txt)
[Week 2](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%202/Week%202.txt)
[Week 3](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%203/Week%203.txt)
[Week 4](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%204/Week%204.txt)
[Week 5](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%205/Week%205.txt)

## IMAT3104 DATABASE MANAGEMENT AND PROGRAMMING NOSQL
### MongoDB Coursework 2022-2023

**1.0 INTRODUCTION**

This is an individual assignment, which gives you an opportunity to demonstrate your knowledge of NoSQL and your ability to implement, query and design a MongoDB document database. You will be awarded marks for what is achieved. This assignment is worth 50% of the overall module mark.

**2.0 SCENARIO**

Goodreads is an American social cataloguing website and a subsidiary of Amazon that allows individuals to search its database of books, annotations, quotes, and reviews. Users can sign up and register books to generate library catalogues and reading lists. They can also create their own groups of book suggestions, surveys, polls, blogs, and discussions (copied directly from the Wikipedia page).
- Goodreads has a web page for each book e.g. https://www.goodreads.com/book/show/5333265
- or each review e.g., https://www.goodreads.com/review/show/2410025795

A subset of Goodreads data has been selected for this coursework. There is data on books, reviews, authors, and genre. Permission was granted to use the data for this coursework from https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home on condition that the authors’ research papers are referenced (Mengting W. and Julian J. M 2018), (Mengting W et al. 2019). It is not necessary to read these papers to complete this coursework.

# **3.0 CONNECT, EXTRACT, TRANSFORM AND LOAD DATA (CETL) [15 marks]**

# 3.1 Connecting to DMU MongoDB server.

Connect to MongoDB server using MongoDB Compass with the following credentials. Please note that the username and password are in lowercase.

| Host                   | Username | Password | Authentication Database |
| ---------------------- | -------- | -------- | ------------------------ |
| mongodb.dmu.ac.uk      | cwuser   | cwuser   | IMAT3104Coursework        |


If you are having difficulties with the credentials above, paste the below URI in the Connection String window

    mongodb://cwuser:cwuser@mongodb.dmu.ac.uk/?authMechanism=DEFAULT&authSource=IMAT3104Coursework

Note: At the moment, you can only connect to the MongoDB server while on university network. This means either you are on the University lab machines or you connect via Horizon. If you wish to work on your PC, you have to complete the ETL using above methods then move to your PC and import the personalized datasets.

# 3.2 Understanding the data.

Study all the collections (read more here: https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home) and answer the following:

1. What is/are the unique/identifying key(s) of the documents of each collection? <b>[4 marks]</b>


Books:


- Books.json:


| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_book_authors | author_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ec0zu-dEyepJpst0nP83ENgBpWn6kxyDkH5Iogn1Mw2dzA) |
| goodreads_book_genres_initial | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EVl07zLePvVOiLQGhNHC8cUB4cw_9qlIz4azALDFhY80Gg) |
| goodreads_book_series | series_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ETuFaUSCFM1FiGn0Re4Q45kBaaN3Ta724tQUhLAL6M5L-Q) |
| goodreads_book_works | work_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EaSAlxEG189NnrUwcrCkJL4BzJavSMO1uxrs3grWmiDL4w) |
| goodreads_books | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EeoY_jKgz01IlfjHqIQY_PoB7DoDxCweL1vpRL1A2vkgMg) |

Genre:

- Genre.Children.json:


| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_children | isbn | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EZ_CixJS7OdPq4fWt8cMX3EB2i51vPMeljoq2XM4Oi0gIA) |
| goodreads_interactions_children | user_id, book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EdAY8z_EuptMqF5gfPyHwpQB9VsEiD-jrzmWz6tXc0ub2A) |
| goodreads_reviews_children | user_id, book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EUYpdXrbz9RBvR36_7cuHjUB9tZLx5MQEZVqzalleuQCEg) |



- Genre.Comics n Graphics:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_comics_graphic | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ETGXmad5ju1Gu_rc7OSX_QgBV3_6NvPsScfOEiu_-sf5Cw) |
| goodreads_interactions_comics_graphic | N/A (No unique key) | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ecy_C1KgS8RKqw84DmA3ruMBpsC8Vx-rkj9Fv1cZ6mRiMQ) |
| goodreads_reviews_comics_graphic | review_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ea1HcuPiXptDiVrLxvAHUzIBWXuWbvsopygvoVfjNxysQw) |


- Genre.Fantasy n Paranormal:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_fantasy_paranormal.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EUgo6_7v6vFKpRDUtPg2qywB2ycEKbPkWiPD16QSgZwAkA) |
| goodreads_interactions_fantasy_paranormal.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EXEqw5clTPdBrGr9Nl2SpS4Bn0qUEyPhiMThPRLHub5BfQ) |
| goodreads_reviews_fantasy_paranormal.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EcwTpxdH1XRHp6qLJVnRFGkBS-HhLIHWPkNtGU9OqAKy3g) |



- Genre.History n Biography:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_history_biography.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EbFFohv1WkhDhtvTOdDCwC0BLbQm8ycynCA8NlGDfxpznA) |
| goodreads_interactions_history_biography.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EckO2Pz8yClMtptbPST46qEB4DzjTW_DzcPZxtU1iwvgsQ) |
| goodreads_reviews_history_biography.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EcERiOQb8CZOhIL5I7s_6WsBTdQO7GelV1OIxhlk49EJwA) |



- Genre.Mystery, Thriller n Crime:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_mystery_thriller_crime.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EQ5tXa0o6LtLhbqpJ5AJvOQBP5IyVrUO8NslerdpED-ZHQ) |
| goodreads_interactions_mystery_thriller_crime.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EcvdawHx6DFPq8HhTorT97sBsPNmV4rOIG13YAabpCNyaw) |
| goodreads_reviews_mystery_thriller_crime.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ERYj0vCw3ylAqu0j5IFXiXcBr0-krwUEIsfFbIpy22M2Wg) |

- Genre.Poetry:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_poetry.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EaiKy_Izyd1GvmbVIHq-aZYBF0OeODCh68f8Kft8HUAU_Q) |
| goodreads_interactions_poetry.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EaKjuHy_5xpFna9Det1Z-REBMW72FQ4IHpZdp7vCGjqzpw) |
| goodreads_reviews_poetry.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EYwV48GcIbxAoLucY0I5z6MBNTqWf2w9-JiEaOg18s-Z7w) |

- Genre.Romance:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_romance.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EXOgdqrOhqZPu1taZwal4MoBwKseTSHPkvwiswho5ou9pQ) |
| goodreads_interactions_romance.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Edsym-cw63lIkNdOBjCx_IIBGbjzIGRcMYSjT8STYzqLBA) |
| goodreads_reviews_romance.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EZSupfmDNy5Lg6feaWiIM-gBzLnvs9XEGQ4B5x9ZToFqGg) |

- Genre.Young Adult:

| Collection | Keys | Link |
| -------- | -------- | -------- |
| goodreads_books_young_adult.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ETMh4zpBK6VDm04RvBackr0BxpUlCygMhn3-wJ9SlFnkmw) |
| goodreads_interactions_young_adult.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ea3oVC_jeVVMhJcVpy4TsQABus-1iWeNiD22pD7ZCGu_uA) |
| goodreads_reviews_young_adult.json | Column | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EeSX9wfLNhBNvpkGLlMh4UgByDFic2H3S80seX-2bAk91g) |


2. Study the collections briefly with their fields and values. <b>[6 marks]</b>
 
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
<i>goodreads_books_children | ex | ex | ex | ex
<i>goodreads_interactions_children | ex | ex | ex | ex
<i>goodreads_reviews_children | ex | ex | ex | ex
<b>Genre.Comics n Graphics:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_comics_graphic | ex | ex | ex | ex
<i>goodreads_interactions_comics_graphic | ex | ex | ex | ex
<i>goodreads_reviews_comics_graphic | ex | ex | ex | ex
<b>Genre.Fantasy n Paranormal:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_fantasy_paranormal.json | ex | ex | ex | ex
<i>goodreads_interactions_fantasy_paranormal.json | ex | ex | ex | ex
<i>goodreads_reviews_fantasy_paranormal.json | ex | ex | ex | ex
<b>Genre.History n Biography:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_history_biography.json | ex | ex | ex | ex
<i>goodreads_interactions_history_biography.json | ex | ex | ex | ex
<i>goodreads_reviews_history_biography.json | ex | ex | ex | ex
<b>Genre.Mystery, Thriller n Crime:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_mystery_thriller_crime.json | ex | ex | ex | ex
<i>goodreads_interactions_mystery_thriller_crime.json | ex | ex | ex | ex
<i>goodreads_reviews_mystery_thriller_crime.json | ex | ex | ex | ex
<b>Genre.Poetry:</b> | <b>=========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_poetry.json | ex | ex | ex | ex
<i>goodreads_interactions_poetry.json | ex | ex | ex | ex
<i>goodreads_reviews_poetry.json | ex | ex | ex | ex
Genre.Romance:</b> | <b>=========</b> | <b>=========</b> | <b>=========</b> | <b>=========</b>
<i>goodreads_books_romance.json | ex | ex | ex | ex
<i>goodreads_interactions_romance.json | ex | ex | ex | ex
<i>goodreads_reviews_romance.json | ex | ex | ex | ex
<b>Genre.Young Adult:</b> | <b>===========</b> | <b>===========</b> | <b>===========</b> | <b>===========</b>
<i>goodreads_books_young_adult.json | ex | ex | ex | ex
<i>goodreads_interactions_young_adult.json | ex | ex | ex | ex
<i>goodreads_reviews_young_adult.json | ex | ex | ex | ex


    
    
    
    
    
    

 

## 3.3 Getting your personalized data. 
<ol>
<li>
Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. <b>[2 mark]</b>

    db.books.find({ assignedGroup: 100 }).forEach(function(doc) {db.p2652259_books.insertOne(doc)})

</li>
<li>
    
Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. <b>[1 mark]</b>

    db.reviews.find({ book_id: { $in: db.p2652259_books.distinct("book_id") } }).forEach(function(doc) {db.p2652259_reviews.insertOne(doc)})

</li>
<li>
    
Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. <b>[1 mark]</b>

    db.authors.find({ author_id: { $in: db.p2652259_books.distinct("author_id") } }).forEach(function(doc) {db.p2652259_authors.insertOne(doc)})

</li>
<li>
    
Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. <b>[1 mark]</b>

    db.genres.aggregate([
      {
        $match: {
          book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
      },
      { $out: "p2652259_genres" }
    ])
    
</li>
</ol>



Export all your collections to your local machine. Then delete all newly created collections matching your
p-number only. Be careful not to delete other students’ collections. Disconnect from the DMU MongoDB
server and connect to localhost server. For those that would want to continue the assessment on their PC,
this is the time to move back to your PC!



## 4.0 CLEANING THE COLLECTIONS <b>[20 marks]</b>

4.1 Have a quick look at the collections on MongoDB Compass, you will realise that some of the data
was only scrapped from the internet and incomplete or not in proper format. Remember, in Section 3.2, you identified some anomalies on the dataset. For each anomaly that happens to be in your personalized dataset:

    
<ol>
1. Provide for a minimum of four (4) anomalies. <b>[4 marks each]</b>

<ol type="a">
<li> Take screenshots of sample documents with the anomaly before it is corrected.</li>


p2652259_reviews:

![Screenshot from 2023-03-10 04-00-50](https://user-images.githubusercontent.com/41834061/224220194-86ff7316-8c8a-4cc4-a57b-3c1829e7ecf8.png)

![Screenshot from 2023-03-10 04-03-15](https://user-images.githubusercontent.com/41834061/224220443-80318186-1fe1-4c94-bf6d-d5196826caa4.png)

![Screenshot from 2023-03-10 04-04-26](https://user-images.githubusercontent.com/41834061/224220535-c701b69b-401b-4762-951f-f6ee2c9e26ba.png)



p2652259_books:

![Screenshot from 2023-03-10 04-06-05](https://user-images.githubusercontent.com/41834061/224220793-f7bcda34-1e53-407e-991c-1a534ff1b1e3.png)

![Screenshot from 2023-03-10 04-07-09](https://user-images.githubusercontent.com/41834061/224220889-e233c243-38e2-488a-b436-d3fce1ceb9c4.png)

![Screenshot from 2023-03-10 04-07-55](https://user-images.githubusercontent.com/41834061/224220983-22ec5ced-7cf4-4ac2-8cc5-21668e2bf2e9.png)

![Screenshot from 2023-03-10 04-08-25](https://user-images.githubusercontent.com/41834061/224221038-6273fe23-c462-40f9-bc82-2b73caff733b.png)

<li> Show the query/queries used to address this anomaly.</li>


db.p2652259_reviews.updateMany(
   { $or: [ { read_at: "" }, { started_at: "" } ] },
   { $set: { read_at: "not given", started_at: "not given" } }
)


p2652259_books:
db.p2652259_books.updateMany(
   { isbn: "" },
   { $set: { isbn: "not specified" } }
)


<li> Take screenshots of samples of documents after the anomaly has been corrected.</li>
</ol>
</ol>
<ol>
2. Create a new field publication_date in the format YYYY-MM-DD that merges publication_day, publication_month, and publication_year <b>[2 marks]</b>
</ol>
<ol>
3. Create another field in the books collection unix_publication_date that converts
publication_date to Unix format. <b>[2 marks]</b>
</ol>


## 5.0 QUERYING THE COLLECTIONS <b>[20 marks]</b>


Write the following queries (Q1 to Q4) against the collections that you personalised and loaded you’re
your local MongoDB. For each query, submit the MongoDB command in plain text, AND present it with its
results as a screenshot showing the command and the documents returned. 

For example:

//Q1001. Find the details of the product identified as <code>" B001KTEBOG "</code>.
<code>db.p77342060_softwares.find({asin:"B001KTEBOG"})</code>

![image](https://user-images.githubusercontent.com/41834061/221639935-5b74f24d-c3d2-48dc-a8f1-a7c49569350f.png)

If a lot of documents are returned, show the first set of documents that are displayed. Ensure that what is
displayed answers the requirements and demonstrates that the query is correct. You must do this to gain
all the marks available for a question.

<ol>

<li> Find top 3 books with highest average rating above 3.5 and are not e-books. Display only book id, title, 
price, and average rating. <b>[5 marks]</b></li> 

<li> Who is the most famous reviewer in your dataset? Most famous reviewer have the highest number 
of 5-rated reviews in your datasets. Display the user_id, average rating, average n_votes, total 
n_comments and total number of reviews written by this reviewer. <b>[6 marks]</b></li> 

<li> Update all reviews written by the most famous reviewer from Q2 by adding a new field named 
most_famous and set its value to true. <b>[4 mark]</b></li> 

<li> Using reviews and books collections, find the title and price of 3 most expensive books reviewed by 
the most famous reviewer. No other product details are required. <b>[5 marks]</b></li> 

</ol>



## 6.0 IMPLEMENT AND EXPLAIN INDEX FOR THE DATABASE <b>[15 marks]</b>

 1. Identify the chosen query from Section 5 and explain/justify your choice of index. Why do you think
 indexing could improve the query. <b>[5 marks]</b>

 2. Implement one index that would improve the querying of the database based on one of the queries
 (Q1-Q4). <b>[2 marks]</b>

 3. Present the execution plan of the selected query before the index and after the index has been
 created. Compare and discuss the execution plans to support your choice and summarise your
 findings. <b>[8 marks]</b>

## 7.0 RE-DESIGN THE DATABASE USING AGGREGATE DATA MODELLING <b>[20 marks]</b>

Write code in MongoDB to automatically embed the details of authors from authors collection and genres
from genres collection with their corresponding book in the books collection. This is an aggregate data
modelling (ADM) question.
Therefore, the books collection will contain the book data, authors data and genre data in a single
collection of books. This requires the following tasks:

 1. Make new copy of the pxxxxxx_books collection and name it pxxxxxx_books_adm. <b>[1 mark]</b>

    db.p2652259_books.aggregate([{$match:{}},{$out:"p2652259_books_adm"}])

 2. Embed all the authors and genres of books into their corresponding book using the new
 pxxxxxx_books_adm collection. <b>[6 marks]</b>

        db.p2652259_books.aggregate([
          {
            $lookup: {
              from: "p2652259_authors",
              localField: "authors",
              foreignField: "author_id",
              as: "authors"
            }
          },
          {
            $lookup: {
              from: "p2652259_genres",
              localField: "book_id",
              foreignField: "book_id",
              as: "genres"
            }
          },
          {
            $project: {
              _id: 1,
              isbn: 1,
              text_reviews_count: 1,
              country_code: 1,
              is_ebook: 1,
              average_rating: 1,
              similar_books: 1,
              description: 1,
              format: 1,
              link: 1,
              publisher: 1,
              num_pages: 1,
              publication_day: 1,
              publication_month: 1,
              edition_information: 1,
              publication_year: 1,
              url: 1,
              image_url: 1,
              book_id: 1,
              ratings_count: 1,
              title: 1,
              unixDateAdded: 1,
              assignedGroup: 1,
              price: 1,
              authors: "$authors",
              genres: "$genres.genres"
            }
          },
          {
            $out: "p2652259_books_adm"
          }
        ])

 3. To verify that the changes to the new products collection were successful, display the title,
 authors, and genre of the book with highest number of pages. <b>[1 mark]</b>



 4. Note that there is no longer need to reference the book_id inside each genre or author_id inside
 each book now that they are part of the book in pxxxxxx_books_adm collection. Show that these
 are removed. <b>[2 marks]</b>


- Genres:

        db.p2652259_genres.bulkWrite([
          { 
            updateMany: { 
              filter: {}, 
              update: { $unset: { book_id: "" } },
              returnNewDocument: true
            } 
          }
        ])

- Authors:

        db.p2652259_authors.bulkWrite([
          { 
            updateMany: { 
              filter: {}, 
              update: { $unset: { author_id: "" } },
              returnNewDocument: true
            } 
          }
        ])


 5. Using $lookup operator, fetch the complete information of 50% of books with their authors and
 genres using pxxxxxxx_books collection. Track the speed of the query. <b>[3 marks]</b>

 6. Using the pxxxxxx_books_adm collection, fetch the same information as above. Track the speed
 of the query. <b>[3 marks]</b>

 7. Compare the performance results of 7(5) & 7(6) above and write a brief discussion about the
 results. <b>[4 marks]</b>


All tasks must each be fully automated by writing code. Note that some tasks may take many seconds or
minutes to execute depending on the machine.

## 8.0 WEEKLY JOURNALS <b>[10 marks]</b>

Report all your journal submissions by completing the table below:

| Week No | Status/Reason                              |
| ------- | ------------------------------------------ |
| 1       | (Submitted on time)            |
| 2       | (Submitted on time)            |
| 3       | (Submitted on time)            |
| 4       | (Submitted on time)            |
| 5       | (Submitted on time)            |



## 9.0 DELIVERABLES
    
<ol>

<li>You are required to upload your answers to the questions into Turnitin on Blackboard as one
PDF file. Do NOT submit any JSON file.

<li>Add your P number to your filename.

<li>Your file must be in a readable format so the NoSQL code in plain text can be copied and executed
from it.

<li>In Turnitin, press both the Upload button and the Confirm button to submit your file and receive
a receipt.
</ol>
    
## 10.0 CLOSING COMMENTS
    
<ol>
    
<li>This is an individual assignment. Do not negotiate with others to clarify what is required. By
uploading your work to Turnitin in Blackboard you will be declaring that the work you submit is
your own and not plagiarised in any way.</li>

<li>Tutors are prepared to offer help with the part of the assignment where there is clear evidence
that you have made a substantial attempt but have become stuck.</li>

<li>Mark totals are shown alongside each question. Your tutor will need to able to execute your NoSQL
code in MongoSH to check for correctness. Marks will be awarded for correctness and
presentation.</li>

<li>Marks will typically be deducted if information is erroneous, missing, irrelevant or difficult to
ascertain. Marks can also be deducted if the answer is particularly inefficient. Therefore, partial
marks are available if an answer is partially correct or partially presented.</li>

<li>There is often more than one way to answer a question and so it is possible to gain full marks for
an answer even if it is different from the markers' specimen set of answers. However, if these
answers do not follow the examples and exercises taught on the module, they may be inferior in some way and so consequently marks may be deducted. Note that some questions hint at the most appropriate method to use.</li>
    
</ol>

## REFERENCES
R. He, J. McAuley. "Modeling the visual evolution of fashion trends with one-class collaborative filtering".
WWW, 2016.
J. McAuley, C. Targett, J. Shi, "A. van den Hengel. Image-based recommendations on styles and substitutes".
SIGIR, 2015.
