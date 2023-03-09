Week Journals:

[Week 1](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%201/Week%201.txt)
[Week 2](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%202/Week%202.txt)
[Week 3](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%203/Week%203.txt)
[Week 4](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%204/Week%204.txt)
[Week 5](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%205/Week%205.txt)


3.0 CONNECT, EXTRACT, TRANSFORM AND LOAD DATA (CETL) <b>[15 marks]</b>

3.1 Connecting to DMU MongoDB server.

    mongodb://cwuser:cwuser@mongodb.dmu.ac.uk/?authMechanism=DEFAULT&authSource=IMAT3104Coursework



3.2 Understanding the data.

1. What is/are the unique/identifying key(s) of the documents of each collection? <b>[4 marks]</b>

2. Study the collections briefly with their fields and values. <b>[6 marks]</b>
 
      a. Identify issues and anomalies you have seen such as invalid data, empty fields, etc.?

      b. Identify examples and how you intend to address them. You do not need to solve the issues here.

Complete the table based on 2(a) and 2(b).


3.3 Getting your personalized data. 
<ol>
<li>
Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. <b>[2 mark]</b>

 

</li>
<li>
    
Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. <b>[1 mark]</b>



</li>
<li>
    
Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. <b>[1 mark]</b>

 

</li>
<li>
    
Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. <b>[1 mark]</b>


</li>
</ol>




4.0 CLEANING THE COLLECTIONS <b>[20 marks]</b>

4.1 Have a quick look at the collections on MongoDB Compass, you will realise that some of the data
was only scrapped from the internet and incomplete or not in proper format. Remember, in Section 3.2, you identified some anomalies on the dataset. For each anomaly that happens to be in your personalized dataset:

    
<ol>
1. Provide for a minimum of four (4) anomalies. <b>[4 marks each]</b>
    
<ol type="a">
<li> Take screenshots of sample documents with the anomaly before it is corrected.</li>
<li> Show the query/queries used to address this anomaly.</li>
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


5.0 QUERYING THE COLLECTIONS <b>[20 marks]</b>


Write the following queries (Q1 to Q4) against the collections that you personalised and loaded you’re
your local MongoDB. For each query, submit the MongoDB command in plain text, AND present it with its
results as a screenshot showing the command and the documents returned. 

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



6.0 IMPLEMENT AND EXPLAIN INDEX FOR THE DATABASE <b>[15 marks]</b>

 1. Identify the chosen query from Section 5 and explain/justify your choice of index. Why do you think
 indexing could improve the query. <b>[5 marks]</b>

 2. Implement one index that would improve the querying of the database based on one of the queries
 (Q1-Q4). <b>[2 marks]</b>

 3. Present the execution plan of the selected query before the index and after the index has been
 created. Compare and discuss the execution plans to support your choice and summarise your
 findings. <b>[8 marks]</b>

7.0 RE-DESIGN THE DATABASE USING AGGREGATE DATA MODELLING <b>[20 marks]</b>

 1. Make new copy of the pxxxxxx_books collection and name it pxxxxxx_books_adm. <b>[1 mark]</b>

 2. Embed all the authors and genres of books into their corresponding book using the new
 pxxxxxx_books_adm collection. <b>[6 marks]</b>

 3. To verify that the changes to the new products collection were successful, display the title,
 authors, and genre of the book with highest number of pages. <b>[1 mark]</b>

 4. Note that there is no longer need to reference the book_id inside each genre or author_id inside
 each book now that they are part of the book in pxxxxxx_books_adm collection. Show that these
 are removed. <b>[2 marks]</b>

 5. Using $lookup operator, fetch the complete information of 50% of books with their authors and
 genres using pxxxxxxx_books collection. Track the speed of the query. <b>[3 marks]</b>

 6. Using the pxxxxxx_books_adm collection, fetch the same information as above. Track the speed
 of the query. <b>[3 marks]</b>

 7. Compare the performance results of 7(5) & 7(6) above and write a brief discussion about the
 results. <b>[4 marks]</b>

8.0 WEEKLY JOURNALS <b>[10 marks]</b>


    
</ol>

REFERENCES

