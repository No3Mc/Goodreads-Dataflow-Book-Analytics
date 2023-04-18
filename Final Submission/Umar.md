p2630030

134

Questions:

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
- book_id : "2008911"

Authors:
- author_id : "137561"

Genres:
- book_id: "21401188", genres: [{"k": "fiction", "v": 7}]

Reviews:
- review_id: "ef77b0cd7d5db0a85aa64f291d63d450"





2. Study the collections briefly with their fields and values. <b>[6 marks]</b>
 
      a. Identify issues and anomalies you have seen such as invalid data, empty fields, etc.?

      b. Identify examples and how you intend to address them. You do not need to solve the issues here.

Complete the table below based on 2(a) and 2(b).


| Collection | Field    | Anomaly      | Examples                   | Solution Plan |
|------------|----------|--------------|-----------------------------|------------------|
| authors   | "name"      | Type Error     | {name: "'ndry@ brw"}   | Update the "name" of the document |
| books   | "format", "publisher", "num_pages", "publication_day", "publication_month", "edition_information", "publication_year"     | Null      | {_id: ObjectId('63e1223417028313a427f9c5')}    | update the "format", "publisher", "num_pages", "publication_day", "publication_month", "edition_information", "publication_year"  of the document |
| genres   | "genre"      | Null      | {_id: ObjectId('63e132d117028313a448d3ab')}     | update the "genre" objects of the document |
| reviews   | "started_at"     | Null      | {_id: ObjectId('63e12b4117028313a44bff12')}     | update the "started_at" to "Not known" of the document |

  

 

## 3.3 Getting your personalized data. 
<ol>
<li>
Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. <b>[2 mark]</b>


    db.books.aggregate([
      { $match: { assignedGroup: 134 } },
      { $out: "p2630030_books" }
    ])


</li>
<li>
    
Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. <b>[1 mark]</b>

    
    db.reviews.aggregate([
    { $match: { book_id: { $in: db.p2630030_books.distinct("book_id") } } },
    { $out: "p2630030_reviews" }
    ]);
 


</li>
<li>
    
Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. <b>[1 mark]</b>

<!--     db.authors.find({ author_id: { $in: db.p2630030_books.distinct("author_id") } }).forEach(function(doc) {db.p2630030_authors.insertOne(doc)}) -->
    
    
    db.authors.aggregate([
    { $match: { author_id: { $in: db.p2630030_books.distinct("authors") } } },
    { $out: "p2630030_authors" }
    ]);

    

</li>
<li>
    
Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. <b>[1 mark]</b>

    db.genres.aggregate([
    { $match: { book_id: { $in: db.p2630030_books.distinct("book_id") } } },
    { $out: "p2630030_genres" }
    ]);

    
    
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

<br>

p2630030_books:





p2630030_reviews:
    



p2630030_genres:
    


p2630030_authors:




<li> Show the query/queries used to address this anomaly.</li>

<br>

p2630030_books:<br>
    
1.

    db.p2630030_books.update(
      {_id: ObjectId("63e1223417028313a427faf6")},
      {
        $set: {
          isbn: "Uncertain",
          description: "Uncertain",
          publication_day: "Uncertain",
          publication_month: "Uncertain",
          publication_year: "Uncertain",
          edition_information: "Uncertain"
        }
      }
    )


1.

    db.p2630030_books.update(
      {_id: ObjectId("63e1223417028313a427fca1")},
      {
        $set: {
          isbn: "Uncertain",
          publisher: "Uncertain",
          publication_day: "Uncertain",
          publication_month: "Uncertain",
          publication_year: "Uncertain" 
        }
      }
    )


1.

    db.p2630030_books.update(
      {_id: ObjectId("63e1223417028313a427fd09")},
      {
        $set: {
          isbn: "Uncertain",
          edition_information: "Uncertain"
        }
      }
    )

1.

    db.p2630030_books.update(
      {_id: ObjectId("63e1223417028313a427fd6b")},
      {
        $set: {
          isbn: "Uncertain",
          publication_day: "Uncertain",
          publication_month: "Uncertain",
          publication_year: "Uncertain",
          edition_information: "Uncertain"
        }
      }
    )

p2630030_reviews:<br>

1.


    db.p2630030_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44bffc9") },
       { $set: { "read_at": "Not Known", "started_at": "Uncertain" } }
    )


1.


    db.p2630030_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44c04e7") },
       { "$set": { "started_at": "Uncertain" } }
    )

1.


    db.p2630030_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44c08a6") },
       { "$set": { "started_at": "Uncertain" } }
    )

1.


    db.p2630030_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44c0c52") },
       { $set: { "read_at": "Uncertain", "started_at": "Uncertain" } }
    )



p2630030_genres:<br>


    
{_id: ObjectId("63e132e117028313a44dc993")}
-1

1.
 
    db.p2630030_genres.updateOne(
       { _id: ObjectId("63e132d117028313a448d4e4") },
       { $set: { genres: [ { k: "Uncertain", v: 0 } ] } }
    )

1.

    db.p2630030_genres.updateOne(
       { _id: ObjectId("63e132d117028313a448d87e") },
       { $set: { genres: [ { k: "Uncertain", v: 0 } ] } }
    )

1.

    db.p2630030_genres.updateOne(
       { _id: ObjectId("63e132d117028313a448de82") },
       { $set: { genres: [ { k: "Uncertain", v: 0 } ] } }
    )

1.

    db.p2630030_genres.updateOne(
       { _id: ObjectId("63e132d117028313a448e1ec") },
       { $set: { genres: [ { k: "Uncertain", v: 0 } ] } }
    )   
    

p2630030_authors:<br>

    {name: 'Julia   Blake'}
    {name: 'Brooke   Lynn'}
    {name: 'Theresa Zomick a/k/a Theresa St. Vincent'}
    {name: 'Ron    Clark'}

    
1.

    db.p2630030_authors.update(
       { _id: ObjectId("63e131fc17028313a44118a6") },
       { $set: { name: "Julia Blake" } }
    )


1.

    db.p2630030_authors.update(
       { _id: ObjectId("63e131fa17028313a4406d17") },
       { $set: { name: "Brooke Lynn" } }
    )

1.

    db.p2630030_authors.update(
       { _id: ObjectId("63e1320417028313a4453276") },
       { $set: { name: "Theresa Zomick" } }
    )

1.

    db.p2630030_authors.update(
       { _id: ObjectId("63e1320217028313a443e1ff") },
       { $set: { name: "Ron Clark" } }
    )




<li> Take screenshots of samples of documents after the anomaly has been corrected.</li>

p2630030_books:





p2630030_reviews:
    



p2630030_genres:
    


p2630030_authors:



    
</ol>
</ol>
<ol>
2. Create a new field publication_date in the format YYYY-MM-DD that merges publication_day, publication_month, and publication_year <b>[2 marks]</b>

Due to the change of N/A the following query should work: <br>


    db.p2630030_books.updateMany(
      {
        $and: [
          { publication_day: { $not: { $in: ["", "Uncertain"] } } },
          { publication_month: { $not: { $in: ["", "Uncertain"] } } },
          { publication_year: { $not: { $in: ["", "Uncertain"] } } }
        ]
      },
      [
        {
          $set: {
            publication_date: {
              $dateFromParts: {
                year: { $toInt: "$publication_year" },
                month: { $toInt: "$publication_month" },
                day: { $toInt: "$publication_day" }
              }
            }
          }
        }
      ]
    )

    
</ol>
<ol>
3. Create another field in the books collection unix_publication_date that converts
publication_date to Unix format. <b>[2 marks]</b>
    
    db.p2630030_books.updateMany(
      {
        publication_date: { $exists: true }
      },
      [
        {
          $set: {
            unix_publication_date: {
              $toLong: { $multiply: [ { $toLong: "$publication_date" }, 1000 ] }
            }
          }
        }
      ]
    )

    
    
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

    db.p2630030_books.aggregate([
    { $match: { is_ebook: "false", average_rating: { $gt: "3.5" } } },
    {
    $group: {
      _id: { book_id: "$book_id", title: "$title", price: "$price" },
      avg_rating: { $avg: { $toDouble: "$average_rating" } },
    },
    },
    { $sort: { avg_rating: -1 } },
    { $limit: 3 },
    {
    $project: {
      _id: "$_id.book_id",
      title: "$_id.title",
      price: "$_id.price",
      avg_rating: 1,
    },
    },
    ])

    
    
<li> Who is the most famous reviewer in your dataset? Most famous reviewer have the highest number 
of 5-rated reviews in your datasets. Display the user_id, average rating, average n_votes, total 
n_comments and total number of reviews written by this reviewer. <b>[6 marks]</b></li> 

    db.p2630030_reviews.aggregate([
    { $match: { rating: 5 } },
    { $group: {
      _id: "$user_id",
      avg_rating: { $avg: "$rating" },
      avg_votes: { $avg: "$n_votes" },
      total_comments: { $sum: "$n_comments" },
      total_reviews: { $sum: 1 }
    } },
    { $sort: { total_reviews: -1 } },
    { $limit: 1 }
    ])

    
    
<li> Update all reviews written by the most famous reviewer from Q2 by adding a new field named 
most_famous and set its value to true. <b>[4 mark]</b></li> 
    
    db.p2630030_reviews.updateMany(
    { user_id: "8e7e5b546a63cb9add8431ee6914cf59" },
    { $set: { most_famous: true } }
    )

    
    
    
<li> Using reviews and books collections, find the title and price of 3 most expensive books reviewed by 
the most famous reviewer. No other product details are required. <b>[5 marks]</b></li> 



    db.p2630030_reviews.aggregate([
      { $match: { user_id: "8e7e5b546a63cb9add8431ee6914cf59" } },
      {
        $lookup: {
          from: "p2630030_books",
          localField: "book_id",
          foreignField: "book_id",
          as: "book"
        }
      },
      { $unwind: "$book" },
      {
        $project: {
          _id: 0,
          title: "$book.title",
          price: "$book.price"
        }
      },
      { $sort: { price: -1 } },
      { $limit: 3 }
    ])

    
    
    
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

        db.p2630030_books.aggregate([{$match:{}},{$out:"p2630030_books_adm"}])

 2. Embed all the authors and genres of books into their corresponding book using the new
 pxxxxxx_books_adm collection. <b>[6 marks]</b>

        db.p2630030_books.aggregate([
          {
            $lookup: {
              from: "p2630030_authors",
              localField: "authors",
              foreignField: "author_id",
              as: "authors"
            }
          },
          {
            $lookup: {
              from: "p2630030_genres",
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
            $out: "p2630030_books_adm"
          }
        ])

 3. To verify that the changes to the new products collection were successful, display the title,
 authors, and genre of the book with highest number of pages. <b>[1 mark]</b>


        db.p2630030_books_adm.aggregate([
          {
            $lookup: {
              from: "p2630030_authors",
              localField: "authors.author_id",
              foreignField: "author_id",
              as: "authors"
            }
          },
          {
            $lookup: {
              from: "p2630030_genres",
              localField: "book_id",
              foreignField: "book_id",
              as: "genres"
            }
          },
          {
            $sort: {
              num_pages: -1
            }
          },
          {
            $limit: 1
          },
          {
            $project: {
              _id: 0,
              title: 1,
              authors: "$authors.name",
              genres: "$genres.genres"
            }
          }
        ])




 4. Note that there is no longer need to reference the book_id inside each genre or author_id inside
 each book now that they are part of the book in pxxxxxx_books_adm collection. Show that these
 are removed. <b>[2 marks]</b>


- Genres:

        db.p2630030_genres.bulkWrite([
          { 
            updateMany: { 
              filter: {}, 
              update: { $unset: { book_id: "" } },
              returnNewDocument: true
            } 
          }
        ])

- Books

        db.p2630030_books.bulkWrite([
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



        db.p2630030_books.aggregate([
          {
            $lookup: {
              from: "p2630030_authors",
              localField: "authors",
              foreignField: "author_id",
              as: "authors"
            }
          },
          {
            $lookup: {
              from: "p2630030_genres",
              localField: "book_id",
              foreignField: "book_id",
              as: "genres"
            }
          },
          {
            $sample: { size: db.p2630030_books.count() / 2 }
          }
        ]).explain("executionStats")



 6. Using the pxxxxxx_books_adm collection, fetch the same information as above. Track the speed
 of the query. <b>[3 marks]</b>


        db.p2630030_books_adm.aggregate([
          {
            $lookup: {
              from: "p2630030_authors",
              localField: "authors",
              foreignField: "author_id",
              as: "authors"
            }
          },
          {
            $lookup: {
              from: "p2630030_genres",
              localField: "book_id",
              foreignField: "book_id",
              as: "genres"
            }
          },
          {
            $sample: { size: db.p2630030_books_adm.count() / 2 }
          }
        ]).explain("executionStats")





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
