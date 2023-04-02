
Weekly journals:

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
- Identifying key: book_id
- Example: "book_id": 5333265

Authors:
- Identifying key: author_id
- Example: "author_id": "604031"

Genres:
- Identifying key: composite key of book_id and genres.k fields
- Example: "book_id": 5333265, "genres": [{"k": "history, historical fiction, biography", "v": 1}]

Reviews:
- Identifying key: review_id
- Example: "review_id": "5cd416f3efc3f944fce4ce2db2290d5e"





2. Study the collections briefly with their fields and values. <b>[6 marks]</b>
 
      a. Identify issues and anomalies you have seen such as invalid data, empty fields, etc.?

      b. Identify examples and how you intend to address them. You do not need to solve the issues here.

Complete the table below based on 2(a) and 2(b).


| Collection | Field    | Anomaly      | Examples                   | Solution Plan |
|------------|----------|--------------|-----------------------------|------------------|
| authors   | name      | Anomaly      | ![Screenshot from 2023-03-30 03-58-17](https://user-images.githubusercontent.com/41834061/228716941-8db8dad6-56bf-40b0-ade6-e4c856e03145.png)     | Solution Plan |
| books   | num-pages      | Null      | ![Screenshot from 2023-03-28 13-52-10](https://user-images.githubusercontent.com/41834061/228242101-fbc7279d-9644-4906-a66e-e8024cf3c4e7.png)     | Solution Plan |
| genres   | fiction      | Mis-Categorization      | ![Screenshot from 2023-03-30 03-50-55](https://user-images.githubusercontent.com/41834061/228715827-16651f72-0243-41d5-93ff-fc80234218b5.png)     | Solution Plan |
| reviews   | read_at, started_at      | Null      | ![Screenshot from 2023-03-29 13-38-31](https://user-images.githubusercontent.com/41834061/228537627-cbde868b-6fdd-42ac-9b68-c641412834d0.png)     | Solution Plan |


    
 To find authors:

    db.authors.aggregate([
      {
        $group: {
          _id: "$name",
          avgRating: { $avg: { $toDouble: "$average_rating" } },
          ratingsCount: { $sum: { $toInt: "$ratings_count" } }
        }
      },
      {
        $match: {
          avgRating: { $gt: 4.5 },
          ratingsCount: { $lt: 100 }
        }
      }
    ])





 To find genres:

    db.genres.find({ "genres.k": "fiction" }).sort({ "genres.v": -1 }).limit(1)
    db.genres.find({ "genres.k": "fiction" }).sort({ "genres.v": 1 }).limit(1)

 The anomaly in this document is the negative value of -87 for the "fiction" genre. This means that there are 87 fewer books categorized under the "fiction" genre for this document than there should be. This could be due to a mis-categorization of the book or a data entry mistake.


    

 

## 3.3 Getting your personalized data. 
<ol>
<li>
Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. <b>[2 mark]</b>


    db.books.aggregate([
      { $match: { assignedGroup: 100 } },
      { $out: "p2652259_books" }
    ])


</li>
<li>
    
Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. <b>[1 mark]</b>

    
    db.reviews.aggregate([
    { $match: { book_id: { $in: db.p2652259_books.distinct("book_id") } } },
    { $out: "p2652259_reviews" }
    ]);
 


</li>
<li>
    
Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. <b>[1 mark]</b>

<!--     db.authors.find({ author_id: { $in: db.p2652259_books.distinct("author_id") } }).forEach(function(doc) {db.p2652259_authors.insertOne(doc)}) -->
    
    
    db.authors.aggregate([
    { $match: { author_id: { $in: db.p2652259_books.distinct("authors") } } },
    { $out: "p2652259_authors" }
    ]);

    

</li>
<li>
    
Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. <b>[1 mark]</b>

    db.genres.aggregate([
    { $match: { book_id: { $in: db.p2652259_books.distinct("book_id") } } },
    { $out: "p2652259_genres" }
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

 To find p2652259_authors:

    db.p2652259_authors.aggregate([
      {
        $group: {
          _id: "$name",
          avgRating: { $avg: { $toDouble: "$average_rating" } },
          ratingsCount: { $sum: { $toInt: "$ratings_count" } }
        }
      },
      {
        $match: {
          avgRating: { $gt: 4.5 },
          ratingsCount: { $lt: 100 }
        }
      }
    ])





 To find p2652259_genres:

    db.p2652259_genres.find({ "genres.k": "fiction" }).sort({ "genres.v": -1 }).limit(1)
    db.p2652259_genres.find({ "genres.k": "fiction" }).sort({ "genres.v": 1 }).limit(1)
    
<ol>
1. Provide for a minimum of four (4) anomalies. <b>[4 marks each]</b>

<ol type="a">
<li> Take screenshots of sample documents with the anomaly before it is corrected.</li>

<br>

p2652259_books:<br>
    
    
    
    db.p2652259_books.updateOne(
       { "_id": ObjectId("63e126d117028313a444d583") },
       { $set: { "publication_year": "2017" } }
    )

    
    
    
{_id: ObjectId("63e1223417028313a427fa45")}<br>
![Screenshot from 2023-03-30 04-31-57](https://user-images.githubusercontent.com/41834061/228721633-a2e2ef8a-f5b9-476d-aba4-78685f98b3b1.png)<br>

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fa45")},
      {
        $set: {
          description: "No description available",
          format: "Unknown format",
          publisher: "Unknown publisher",
          num_pages: "N/A",
          publication_day: "N/A",
          publication_month: "N/A",
          publication_year: "N/A"
        }
      }
    )



{_id: ObjectId("63e1223417028313a427fa78")} <br>
![Screenshot from 2023-03-30 04-32-55](https://user-images.githubusercontent.com/41834061/228721733-9336b3da-1763-4f04-8b57-d0bc6ea287c7.png)<br>

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fa78")},
      {$set: {isbn: "N/A"}}
    )


{_id: ObjectId("63e1223417028313a427fafd")} <br>
![Screenshot from 2023-03-30 04-33-13](https://user-images.githubusercontent.com/41834061/228721829-bbb1179a-7ce0-4afb-aea9-10775e1963ce.png)<br>


    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fafd")},
      {$set: {edition_information: "N/A"}}
    )


{_id: ObjectId("63e1223417028313a427fb42")} <br>
![Screenshot from 2023-03-30 04-33-52](https://user-images.githubusercontent.com/41834061/228721854-1b3d33cc-c38d-4d32-b857-7ea9700de807.png)

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fb42")},
      {
        $set: {
          isbn: "N/A",
          description: "No description available",
          format: "Unknown format",
          publisher: "Unknown publisher",
          edition_information: "N/A",
          num_pages: "N/A",
          publication_day: "N/A",
          publication_month: "N/A",
          publication_year: "N/A"
        }
      }
    )


p2652259_reviews:<br>
{_id: ObjectId("63e12b4117028313a44c0879")} <br>
![Screenshot from 2023-03-30 04-35-17](https://user-images.githubusercontent.com/41834061/228722078-a2e64ae6-4cf5-4430-bbde-ee77e2e1112d.png)<br>

    db.p2652259_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44c0879") },
       { $set: { "read_at": "Not Known", "started_at": "Not Known" } }
    )


{_id: ObjectId("63e12b4117028313a44c106b")} <br>
![Screenshot from 2023-03-30 04-35-53](https://user-images.githubusercontent.com/41834061/228722147-0a68dc10-0ad1-4861-904b-aa48c0b2abe9.png)<br>

    db.p2652259_reviews.updateOne(
      { "_id": ObjectId("63e12b4117028313a44c106b") },
      { "$set": { "started_at": "Not Known" } }
    );


{_id: ObjectId("63e12b4117028313a44c11e5")} <br>
![Screenshot from 2023-03-30 04-36-06](https://user-images.githubusercontent.com/41834061/228722186-09937da5-72c4-4fcb-8bfb-a17357d12dc9.png)<br>

    db.p2652259_reviews.updateOne(
       { "review_id": "42ea1874bf4c32fbaee703161e598177" },
       { "$set": { "started_at": "Not Known" } }
    )


{_id: ObjectId("63e12b4217028313a44c128d")} <br>
![Screenshot from 2023-03-30 04-36-46](https://user-images.githubusercontent.com/41834061/228722289-dd3bdd83-4793-4f97-886b-67613ab15992.png)

    db.p2652259_reviews.update(
      { "_id": ObjectId("63e12b4217028313a44c128d") },
      { "$set": { "started_at": "Not Known" } }
    )


p2652259_genres:<br>

{_id: ObjectId("63e132d117028313a448d433")} <br>
![Screenshot from 2023-03-31 05-19-52](https://user-images.githubusercontent.com/41834061/229021793-4d101976-10c7-4005-b48e-d384ad47ec9b.png)<br>

    db.p2652259_genres.updateOne(
       { _id: "63e132d117028313a448d433" },
       { $set: { genres: [ { k: "unknown", v: 0 } ] } }
    )
    


p2652259_authors:<br>

{name: 'nise7465'} <br>
![Screenshot from 2023-03-30 04-39-07](https://user-images.githubusercontent.com/41834061/228722663-a68250d6-1ad3-4501-9464-282c0f40fb42.png)<br>

    db.p2652259_authors.update(
       { _id: ObjectId("63e1320417028313a4450356") },
       { $set: { name: "Nise" } }
    )


{name: 'fdy `dl@'} <br>
![Screenshot from 2023-03-30 04-40-52](https://user-images.githubusercontent.com/41834061/228722871-5eaa0fdf-9914-47e0-8403-9a9e490d2b0d.png)<br>

    db.p2652259_authors.update(
       { _id: ObjectId("63e131f517028313a43e557b") },
       { $set: { name: "Fdy Dl" } }
    )


{name: 'Susan    Scott'} 4 char space<br>
![Screenshot from 2023-03-30 04-42-06](https://user-images.githubusercontent.com/41834061/228723024-b922507f-8049-4e05-ac69-9e14120590bf.png)<br>

    db.p2652259_authors.update(
       { _id: ObjectId("63e1320117028313a443aecd") },
       { $set: { name: "Susan Scott" } }
    )


{name: 'hwyd s`yd'} <br>
![Screenshot from 2023-03-30 04-28-54](https://user-images.githubusercontent.com/41834061/228721301-eaec0cc7-d146-424e-a6d2-9670fc0d9534.png)

    db.p2652259_authors.update(
       { _id: ObjectId("63e131ff17028313a4429338") },
       { $set: { name: "Hwyd Syd" } }
    )



<li> Show the query/queries used to address this anomaly.</li>

<br>

p2652259_books:

1.

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fa45")},
      {
        $set: {
          description: "No description available",
          format: "Unknown format",
          publisher: "Unknown publisher",
          num_pages: "N/A",
          publication_day: "N/A",
          publication_month: "N/A",
          publication_year: "N/A"
        }
      }
    )

2.

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fa78")},
      {$set: {isbn: "N/A"}}
    )


3.


    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fafd")},
      {$set: {edition_information: "N/A"}}
    )
    

4.
  

    db.p2652259_books.update(
      {_id: ObjectId("63e1223417028313a427fb42")},
      {
        $set: {
          isbn: "N/A",
          description: "No description available",
          format: "Unknown format",
          publisher: "Unknown publisher",
          edition_information: "N/A",
          num_pages: "N/A",
          publication_day: "N/A",
          publication_month: "N/A",
          publication_year: "N/A"
        }
      }
    )

p2652259_reviews:

1. 


    db.p2652259_reviews.updateOne(
       { "_id": ObjectId("63e12b4117028313a44c0879") },
       { $set: { "read_at": "Not Known", "started_at": "Not Known" } }
    )


2.

    db.p2652259_reviews.updateOne(
      { "_id": ObjectId("63e12b4117028313a44c106b") },
      { "$set": { "started_at": "Not Known" } }
    );


3.  


    db.p2652259_reviews.updateOne(
       { "review_id": "42ea1874bf4c32fbaee703161e598177" },
       { "$set": { "started_at": "Not Known" } }
    )


4.


    db.p2652259_reviews.update(
      { "_id": ObjectId("63e12b4217028313a44c128d") },
      { "$set": { "started_at": "Not Known" } }
    )


p2652259_genres:

    db.p2652259_genres.updateOne(
       { _id: "63e132d117028313a448d433" },
       { $set: { genres: [ { k: "unknown", v: 0 } ] } }
    )


p2652259_authors:
     
1.


    db.p2652259_authors.update(
       { _id: ObjectId("63e1320417028313a4450356") },
       { $set: { name: "Nise" } }
    )


2.


    db.p2652259_authors.update(
       { _id: ObjectId("63e131f517028313a43e557b") },
       { $set: { name: "Fdy Dl" } }
    )


3.


    db.p2652259_authors.update(
       { _id: ObjectId("63e1320117028313a443aecd") },
       { $set: { name: "Susan Scott" } }
    )


4.


    db.p2652259_authors.update(
       { _id: ObjectId("63e131ff17028313a4429338") },
       { $set: { name: "Hwyd Syd" } }
    )


<li> Take screenshots of samples of documents after the anomaly has been corrected.</li>

p2652259_books:

![Screenshot from 2023-03-31 15-52-49](https://user-images.githubusercontent.com/41834061/229155151-f29dac9b-1551-4a09-9b33-a473f4bbcab1.png)
![Screenshot from 2023-03-31 15-55-19](https://user-images.githubusercontent.com/41834061/229155659-8fbfe53c-03c0-435d-9589-c38fafd75626.png)
![Screenshot from 2023-03-31 15-56-18](https://user-images.githubusercontent.com/41834061/229155934-e6f79919-8cf2-43b6-ae6b-768af6ef4542.png)
![Screenshot from 2023-03-31 16-29-57](https://user-images.githubusercontent.com/41834061/229164318-d2916e0f-c042-446a-bf3d-aece3298114e.png)



p2652259_reviews:
    
![Screenshot from 2023-03-31 16-07-19](https://user-images.githubusercontent.com/41834061/229158551-e5a99f53-3d5d-402e-9d21-032bcd2e6e91.png)
![Screenshot from 2023-03-31 16-08-02](https://user-images.githubusercontent.com/41834061/229158666-d13aa62a-b427-4da9-9568-fd2818e48174.png)
![Screenshot from 2023-03-31 16-08-34](https://user-images.githubusercontent.com/41834061/229158791-886af0df-484b-4f90-89e0-b8851e85e049.png)
![Screenshot from 2023-03-31 16-09-23](https://user-images.githubusercontent.com/41834061/229158977-09b9e2fb-9ea7-4d52-9d8d-00f99e53ceb0.png)


p2652259_genres:
    


p2652259_authors:

![Screenshot from 2023-03-31 16-15-16](https://user-images.githubusercontent.com/41834061/229160430-0dc6e0dc-4975-4e6e-9cd8-d5c8fc96ad49.png)
![Screenshot from 2023-03-31 16-23-26](https://user-images.githubusercontent.com/41834061/229162849-c901aeeb-ff83-4bdf-b6e7-1be18d529f60.png)
![Screenshot from 2023-03-31 16-19-33](https://user-images.githubusercontent.com/41834061/229161744-9a714e14-5947-4488-a1e9-956d297153f7.png)
![Screenshot from 2023-03-31 16-22-05](https://user-images.githubusercontent.com/41834061/229162371-863bdc32-25b2-4889-8c6c-1dbaaa45a156.png)

    
</ol>
</ol>
<ol>
2. Create a new field publication_date in the format YYYY-MM-DD that merges publication_day, publication_month, and publication_year <b>[2 marks]</b>

Due to the change of N/A the following query should work: <br>


    db.p2652259_books.updateMany(
      {
        $and: [
          { publication_day: { $not: { $in: ["", "N/A"] } } },
          { publication_month: { $not: { $in: ["", "N/A"] } } },
          { publication_year: { $not: { $in: ["", "N/A"] } } }
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
    
    db.p2652259_books.updateMany(
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

    db.p2652259_books.aggregate([
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

    db.p2652259_reviews.aggregate([
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
    
    db.p2652259_reviews.updateMany(
    { user_id: "9c692e44fab3d5ca585cf282344f18e1" },
    { $set: { most_famous: true } }
    )

    
    
    
<li> Using reviews and books collections, find the title and price of 3 most expensive books reviewed by 
the most famous reviewer. No other product details are required. <b>[5 marks]</b></li> 



    db.p2652259_reviews.aggregate([
      { $match: { user_id: "9c692e44fab3d5ca585cf282344f18e1" } },
      {
        $lookup: {
          from: "p2652259_books",
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
