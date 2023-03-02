## IMAT3104 DATABASE MANAGEMENT AND PROGRAMMING NOSQL
### MongoDB Coursework 2022-2023

**1.0 INTRODUCTION**

This is an individual assignment, which gives you an opportunity to demonstrate your knowledge of NoSQL and your ability to implement, query and design a MongoDB document database. You will be awarded marks for what is achieved. This assignment is worth 50% of the overall module mark.

--------


**2.0 SCENARIO**

Goodreads is an American social cataloguing website and a subsidiary of Amazon that allows individuals to search its database of books, annotations, quotes, and reviews. Users can sign up and register books to generate library catalogues and reading lists. They can also create their own groups of book suggestions, surveys, polls, blogs, and discussions (copied directly from the Wikipedia page).
- Goodreads has a web page for each book e.g. https://www.goodreads.com/book/show/5333265
- or each review e.g., https://www.goodreads.com/review/show/2410025795

A subset of Goodreads data has been selected for this coursework. There is data on books, reviews, authors, and genre. Permission was granted to use the data for this coursework from https://sites.google.com/eng.ucsd.edu/ucsdbookgraph/home on condition that the authors’ research papers are referenced (Mengting W. and Julian J. M 2018), (Mengting W et al. 2019). It is not necessary to read these papers to complete this coursework.

---------


**3.0 CONNECT, EXTRACT, TRANSFORM AND LOAD DATA (CETL) [15 marks]**

3.1 Connecting to DMU MongoDB server.

Connect to MongoDB server using MongoDB Compass with the following credentials. Please note that the username and password are in lowercase.

| Host                   | Username | Password | Authentication Database |
| ---------------------- | -------- | -------- | ------------------------ |
| mongodb.dmu.ac.uk      | cwuser   | cwuser   | IMAT3104Coursework        |


If you are having difficulties with the credentials above, paste the below URI in the Connection String window

mongodb://cwuser:cwuser@mongodb.dmu.ac.uk/?authMechanism=DEFAULT&authSource=IMAT3104Coursework

Note: At the moment, you can only connect to the MongoDB server while on university network. This means either you are on the University lab machines or you connect via Horizon. If you wish to work on your PC, you have to complete the ETL using above methods then move to your PC and import the personalized datasets.

-------


3.2




1. What is/are the unique/identifying key(s) of the documents of each collection? [4 marks]

Books:


- Books.json:


| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| goodreads_book_authors | author_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ec0zu-dEyepJpst0nP83ENgBpWn6kxyDkH5Iogn1Mw2dzA) |
| goodreads_book_genres_initial | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EVl07zLePvVOiLQGhNHC8cUB4cw_9qlIz4azALDFhY80Gg) |
| goodreads_book_series | series_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ETuFaUSCFM1FiGn0Re4Q45kBaaN3Ta724tQUhLAL6M5L-Q) |
| goodreads_book_works | work_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EaSAlxEG189NnrUwcrCkJL4BzJavSMO1uxrs3grWmiDL4w) |
| goodreads_books | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EeoY_jKgz01IlfjHqIQY_PoB7DoDxCweL1vpRL1A2vkgMg) |

Genre:

- Genre.Children.json:


| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| goodreads_books_children | isbn | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EZ_CixJS7OdPq4fWt8cMX3EB2i51vPMeljoq2XM4Oi0gIA) |
| goodreads_interactions_children | user_id, book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EdAY8z_EuptMqF5gfPyHwpQB9VsEiD-jrzmWz6tXc0ub2A) |
| goodreads_reviews_children | user_id, book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/EUYpdXrbz9RBvR36_7cuHjUB9tZLx5MQEZVqzalleuQCEg) |



- Genre.Comics n Graphics:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| goodreads_books_comics_graphic | book_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/ETGXmad5ju1Gu_rc7OSX_QgBV3_6NvPsScfOEiu_-sf5Cw) |
| goodreads_interactions_comics_graphic | N/A (No unique key) | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ecy_C1KgS8RKqw84DmA3ruMBpsC8Vx-rkj9Fv1cZ6mRiMQ) |
| goodreads_reviews_comics_graphic | review_id | [Link](https://demontfortuniversity-my.sharepoint.com/:u:/g/personal/p2652259_my365_dmu_ac_uk/Ea1HcuPiXptDiVrLxvAHUzIBWXuWbvsopygvoVfjNxysQw) |


- Genre.Fantasy n Paranormal:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |



- Genre.History n Biography:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |



- Genre.Mystery, Thriller n Crime:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |

- Genre.Poetry:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |

- Genre.Romance:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |

- Genre.Young Adult:

| Collection | Primary Key | Link |
| -------- | -------- | -------- |
| Column | Column | [Link]() |
| Column | Column | [Link]() |
| Column | Column | [Link]() |

----------




3.3 Getting your personalized data. 

1.Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. [2 mark]

    db.books.find({ assignedGroup: 100 }).forEach(function(doc) {db.p2652259_books.insertOne(doc)})

2.Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. [1 mark]

    db.reviews.find({ book_id: { $in: db.p2652259_books.distinct("book_id") } }).forEach(function(doc) {db.p2652259_reviews.insertOne(doc)})

3.Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. [1 mark]

    db.authors.find({ author_id: { $in: db.p2652259_books.distinct("author_id") } }).forEach(function(doc) {db.p2652259_authors.insertOne(doc)})

4.Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. [1 mark]

    db.genres.aggregate([
      {
        $match: {
          book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
      },
      { $out: "p2652259_genres" }
    ])

------------
