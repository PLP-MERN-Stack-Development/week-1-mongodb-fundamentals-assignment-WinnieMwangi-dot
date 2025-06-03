. **Install** MongoDB Community Edition or set up a MongoDB Atlas cluster.  
2. Open **terminal** or **command prompt**.  
3. Start the Mongo shell by running:  
   ```bash
   mongosh
Switch to the database:

js
Copy
Edit
use plp_bookstore
Insert sample book data:

Open insert_books.js in a text editor.

Copy the entire db.books.insertMany([...]) command.

Paste it into the mongosh prompt and run.

insert_books.js Sample Content
js
Copy
Edit
db.books.insertMany([
  {
    title: "Book One",
    author: "Author A",
    genre: "Fiction",
    published_year: 2012,
    price: 15.99,
    in_stock: true,
    pages: 320,
    publisher: "Publisher X"
  },
  {
    title: "Book Two",
    author: "Author B",
    genre: "Science",
    published_year: 2015,
    price: 18.99,
    in_stock: false,
    pages: 250,
    publisher: "Publisher Y"
  },
  {
    title: "Book Three",
    author: "Author C",
    genre: "Fantasy",
    published_year: 2008,
    price: 22.5,
    in_stock: true,
    pages: 400,
    publisher: "Publisher Z"
  },
  {
    title: "Book Four",
    author: "Author A",
    genre: "Fiction",
    published_year: 2020,
    price: 19.99,
    in_stock: true,
    pages: 310,
    publisher: "Publisher X"
  },
  {
    title: "Book Five",
    author: "Author D",
    genre: "History",
    published_year: 2011,
    price: 12.99,
    in_stock: false,
    pages: 280,
    publisher: "Publisher W"
  },
  {
    title: "Book Six",
    author: "Author E",
    genre: "Biography",
    published_year: 2013,
    price: 16.0,
    in_stock: true,
    pages: 350,
    publisher: "Publisher Q"
  },
  {
    title: "Book Seven",
    author: "Author F",
    genre: "Mystery",
    published_year: 2001,
    price: 14.5,
    in_stock: true,
    pages: 270,
    publisher: "Publisher V"
  },
  {
    title: "Book Eight",
    author: "Author G",
    genre: "Science",
    published_year: 2019,
    price: 20.0,
    in_stock: true,
    pages: 310,
    publisher: "Publisher Y"
  },
  {
    title: "Book Nine",
    author: "Author A",
    genre: "Fiction",
    published_year: 2005,
    price: 10.0,
    in_stock: false,
    pages: 290,
    publisher: "Publisher X"
  },
  {
    title: "Book Ten",
    author: "Author H",
    genre: "Fantasy",
    published_year: 2022,
    price: 25.0,
    in_stock: true,
    pages: 420,
    publisher: "Publisher Z"
  }
])
Sample Queries (queries.js)
Basic CRUD Operations
js
Copy
Edit
// Find all books in a specific genre
db.books.find({ genre: "Fiction" }).pretty()

// Find books published after a certain year
db.books.find({ published_year: { $gt: 2015 } }).pretty()

// Find books by a specific author
db.books.find({ author: "Author A" }).pretty()

// Update the price of a specific book
db.books.updateOne(
  { title: "Book One" },
  { $set: { price: 17.99 } }
)

// Delete a book by its title
db.books.deleteOne({ title: "Book Nine" })
Advanced Queries
js
Copy
Edit
// Books in stock and published after 2010
db.books.find({ in_stock: true, published_year: { $gt: 2010 } }).pretty()

// Projection: only title, author, price
db.books.find(
  { genre: "Science" },
  { title: 1, author: 1, price: 1, _id: 0 }
)

// Sort books by price ascending
db.books.find().sort({ price: 1 }).pretty()

// Sort books by price descending
db.books.find().sort({ price: -1 }).pretty()

// Pagination: page 1 (first 5 books)
db.books.find().skip(0).limit(5).pretty()

// Pagination: page 2 (next 5 books)
db.books.find().skip(5).limit(5).pretty()
Aggregation Pipelines
js
Copy
Edit
// Average price of books by genre
db.books.aggregate([
  { $group: { _id: "$genre", avgPrice: { $avg: "$price" } } }
])

// Author with the most books
db.books.aggregate([
  { $group: { _id: "$author", count: { $sum: 1 } } },
  { $sort: { count: -1 } },
  { $limit: 1 }
])

// Group books by publication decade and count
db.books.aggregate([
  {
    $group: {
      _id: {
        $concat: [
          { $toString: { $multiply: [{ $floor: { $divide: ["$published_year", 10] } }, 10] } },
          "s"
        ]
      },
      count: { $sum: 1 }
    }
  }
])
Indexing and Performance
js
Copy
Edit
// Create index on title
db.books.createIndex({ title: 1 })

// Create compound index on author and published_year
db.books.createIndex({ author: 1, published_year: -1 })

// Check query performance with explain()
db.books.find({ title: "Book One" }).explain("executionStats")
db.books.find({ author: "Author A", published_year: { $gt: 2010 } }).explain("executionStats")
How to Run
Start mongosh and connect to your MongoDB instance.

Switch to your database:

js
Copy
Edit
use plp_bookstore
Copy and paste the queries from this file or your queries.js into the shell.

Use MongoDB Compass to visually verify data and indexes.
