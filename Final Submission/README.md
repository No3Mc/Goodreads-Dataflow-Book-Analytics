3.3 Getting your personalized data. 


####    Extract all books matching your assignedGroup from books collection into a new collection named pxxxxxxx_books where pxxxxxxx is replaced with your p-number. [2 mark]

####    Extract all matching reviews of books in pxxxxxxx_books to a collection named pxxxxxxx_reviews. [1 mark]

####   Extract all matching authors of books in pxxxxxxx_books to a collection named pxxxxxxx_authors. [1 mark]

####    Extract all matching genres of books in pxxxxxxx_books to a new collection named pxxxxxxx_genres. [1 mark]


1.

    db.books.find({ assignedGroup: 100 }).forEach(function(doc) {db.p2652259_books.insertOne(doc)})

2.

    db.reviews.find({ book_id: { $in: db.p2652259_books.distinct("book_id") } }).forEach(function(doc) {db.p2652259_reviews.insertOne(doc)})

3.

    db.authors.find({ author_id: { $in: db.p2652259_books.distinct("author_id") } }).forEach(function(doc) {db.p2652259_authors.insertOne(doc)})

4.

    db.genres.aggregate([
      {
        $match: {
          book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
      },
      { $out: "p2652259_genres" }
    ])

------------

<!-- 1.  -->

<!--     db.books.aggregate([
    { $match: { assignedGroup: 100 } },
    { $out: "p2652259_books" }
    ]) -->





<!-- 2.  -->

<!--     db.reviews.aggregate([
    {
        $match: {
        book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
    },
    { $out: "p2652259_reviews" }
    ]) -->




<!-- 2. db.p2652259_books.aggregate([
  { $lookup: {
      from: "reviews",
      localField: "book_id",
      foreignField: "book_id",
      as: "reviews"
  } },
  { $unwind: "$reviews" },
  { $project: { _id: 0 } }, 
  { $out: "p2652259_reviews" }
]) -->







<!-- 3. -->
<!-- 
    db.authors.aggregate([
    {
        $match: {
        author_id: { $in: db.p2652259_books.distinct("authors") }
        }
    },
    { $out: "p2652259_authors" }
    ]) -->



<!-- 4. -->

<!--     db.genres.aggregate([
    {
        $match: {
        book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
    },
    { $out: "p2652259_genres" }
    ]) -->



