#Backend Code
#Route: routes/books.js
// GET /api/books
router.get('/', searchBooks);

#Controller: controllers/bookController.js
exports.searchBooks = async (req, res) => {
  try {
    const { genre, title } = req.query;
    const query = {};

    if (genre) query.genre = genre;
    if (title) query.title = { $regex: title, $options: 'i' }; // Case-insensitive search

    const books = await Book.find(query);
    if (books.length === 0) {
      return res.status(404).json({ message: 'No books found' });
    }

    res.status(200).json(books);
  } catch (error) {
    res.status(500).json({ error: 'Failed to fetch books' });
  }
};