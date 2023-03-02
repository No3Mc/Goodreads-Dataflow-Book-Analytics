3.3 Getting your personalized data. 


1. 

    db.books.aggregate([
    { $match: { assignedGroup: 100 } },
    { $out: "p2652259_books" }
    ])


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


2. 

    db.reviews.aggregate([
    {
        $match: {
        author_id: { $in: db.p2652259_books.distinct("reviews") }
        }
    },
    { $out: "p2652259_reviews" }
    ])


3.

    db.authors.aggregate([
    {
        $match: {
        author_id: { $in: db.p2652259_books.distinct("authors") }
        }
    },
    { $out: "p2652259_authors" }
    ])



4.

    db.genres.aggregate([
    {
        $match: {
        book_id: { $in: db.p2652259_books.distinct("book_id") }
        }
    },
    { $out: "p2652259_genres" }
    ])



