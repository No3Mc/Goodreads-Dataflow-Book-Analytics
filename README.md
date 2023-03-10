Weekly Journals:
[Week 1](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%201/Week%201.txt) ✅
[Week 2](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%202/Week%202.txt) ✅
[Week 3](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%203/Week%203.txt) ✅
[Week 4](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%204/Week%204.txt) ✅
[Week 5](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Journals/Week%205/Week%205.txt) ✅



3.0 [CONNECT, EXTRACT, TRANSFORM AND LOAD DATA (CETL)](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#30-connect-extract-transform-and-load-data-cetl-15-marks) 

3.2 Understanding the data

What is/are the unique/identifying key(s) of the documents of each collection? [4 marks]

Study the collections briefly with their fields and values. [6 marks]

4.0 [CLEANING THE COLLECTIONS](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#40-cleaning-the-collections-20-marks)

4.1
    c. Take screenshots of samples of documents after the anomaly has been corrected.

5.0 [QUERYING THE COLLECTIONS](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#50-querying-the-collections-20-marks)

6.0 [IMPLEMENT AND EXPLAIN INDEX FOR THE DATABASE](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#60-implement-and-explain-index-for-the-database-15-marks)

7.0 [RE-DESIGN THE DATABASE USING AGGREGATE DATA MODELLING](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#80-weekly-journals-10-marks)

To verify that the changes to the new products collection were successful, display the title, authors, and genre of the book with highest number of pages. [1 mark]

Using $lookup operator, fetch the complete information of 50% of books with their authors and genres using pxxxxxxx_books collection. Track the speed of the query. [3 marks]

Using the pxxxxxx_books_adm collection, fetch the same information as above. Track the speed of the query. [3 marks]

Compare the performance results of 7(5) & 7(6) above and write a brief discussion about the results. [4 marks]


8.0 [WEEKLY JOURNALS](https://github.com/No3Mc/NOSQL-DMnP/blob/main/Final%20Submission/README.md#80-weekly-journals-10-marks)  ✅

P2652259_authors:

    {
      "_id": {
        "$oid": "63e131f017028313a43c2b45"
      },
      "average_rating": "3.92",
      "author_id": "10333",
      "text_reviews_count": "5075",
      "name": "Barbara Hambly",
      "ratings_count": "122118"
    }

P2652259_books:

    {
      "_id": {
        "$oid": "63e1223417028313a427fa45"
      },
      "isbn": "0896211371",
      "text_reviews_count": "1",
      "country_code": "US",
      "is_ebook": "false",
      "average_rating": "3.67",
      "similar_books": [],
      "description": "",
      "format": "",
      "link": "https://www.goodreads.com/book/show/1531571.Family_Life",
      "authors": "714123",
      "publisher": "",
      "num_pages": "",
      "publication_day": "",
      "publication_month": "",
      "edition_information": "Large Print",
      "publication_year": "",
      "url": "https://www.goodreads.com/book/show/1531571.Family_Life",
      "image_url": "https://s.gr-assets.com/assets/nophoto/book/111x148-bcc042a9c91a29c1d680899eff700a03.png",
      "book_id": 1531571,
      "ratings_count": "6",
      "title": "Family Life",
      "unixDateAdded": 1333891862,
      "assignedGroup": 100,
      "price": "$353.252",
      "author_id": "714123"
    }

p2652259_genres:

    {
      "_id": {
        "$oid": "63e132d117028313a448d433"
      },
      "book_id": 1531571,
      "genres": []
    }


p2652259_reviews:

    {
      "_id": {
        "$oid": "63e12b7a17028313a4553a73"
      },
      "user_id": "62e739fa9c4d4f6899bb9597780f0ead",
      "book_id": 248,
      "review_id": "98f93fa952213acfda9b64b5ac8927e9",
      "rating": 4,
      "review_text": "Second time around and finding it just as interesting, perhaps even more so, that the first reading some forty years ago.",
      "date_added": "Thu Sep 06 17:56:32 -0700 2012",
      "date_updated": "Tue Mar 26 14:24:06 -0700 2013",
      "read_at": "Mon Oct 01 00:00:00 -0700 2012",
      "started_at": "Thu Sep 06 00:00:00 -0700 2012",
      "n_votes": 1,
      "n_comments": 0
    }


p2652259_books_adm:

        {
          "_id": {
            "$oid": "63e1223517028313a42801f5"
          },
          "isbn": "1904095712",
          "text_reviews_count": "1",
          "country_code": "US",
          "is_ebook": "false",
          "average_rating": "3.53",
          "similar_books": [],
          "description": "In 1919 Sian Busby's great-grandmother, Beth, gave birth to triplets. One of the babies died at birth and 11 days later Beth drowned the surviving twins in a bath of cold water. She was sentenced to an indefinite term of imprisonment at Broadmoor.The murder and the deep sense of shame it generated obviously affected Beth, her husband and their surviving children to an extraordinary degree, but it also resounded through the lives of her grandchildren and great-grandchildren. It gave rise to a collective anxiety about their family's ability to parent and an obsessive fear of hereditary insanity and depressive illness.In Sian's case, ill-suppressed knowledge of the event manifested itself in recurring nightmares and contributed to a prolonged bout of post-natal depression. She decided to investigate the story once and for all and lay to rest the ghosts which have haunted the family for 80 years.",
          "format": "Hardcover",
          "link": "https://www.goodreads.com/book/show/1911901.The_Cruel_Mother",
          "publisher": "Short Books",
          "num_pages": "301",
          "publication_day": "1",
          "publication_month": "1",
          "edition_information": "",
          "publication_year": "2004",
          "url": "https://www.goodreads.com/book/show/1911901.The_Cruel_Mother",
          "image_url": "https://s.gr-assets.com/assets/nophoto/book/111x148-bcc042a9c91a29c1d680899eff700a03.png",
          "book_id": 1911901,
          "ratings_count": "9",
          "title": "The Cruel Mother: A Family Ghost Laid to Rest",
          "unixDateAdded": 1331924445,
          "assignedGroup": 100,
          "price": "$176.492",
          "authors": [
            {
              "_id": {
                "$oid": "63e131f017028313a43c3687"
              },
              "average_rating": "3.37",
              "author_id": "169908",
              "text_reviews_count": "131",
              "name": "Sian Busby",
              "ratings_count": "964"
            }
          ],
          "genres": [
            [
              {
                "k": "non-fiction",
                "v": 6
              },
              {
                "k": "history, historical fiction, biography",
                "v": 6
              },
              {
                "k": "mystery, thriller, crime",
                "v": 3
              }
            ]
          ]
        }
