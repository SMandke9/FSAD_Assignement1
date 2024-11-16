#User Story 1
#Backend code
#Route: routes/books.js
const express = require('express');
const router = express.Router();
const { addBook } = require('../controllers/bookController');

// POST /api/books
router.post('/', addBook);

module.exports = router;

#Controller: controllers/bookController.js
const Book = require('../models/Book');

// Add a book
exports.addBook = async (req, res) => {
  try {
    const { title, author, genre, condition, ownerId } = req.body;

    if (!title || !author || !genre || !condition || !ownerId) {
      return res.status(400).json({ error: 'All fields are required' });
    }

    const newBook = new Book({ title, author, genre, condition, ownerId });
    const savedBook = await newBook.save();

    res.status(201).json(savedBook);
  } catch (error) {
    res.status(500).json({ error: 'Failed to add book' });
  }
};

#Model: models/Book.js
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  title: { type: String, required: true },
  author: { type: String, required: true },
  genre: { type: String, required: true },
  condition: { type: String, required: true },
  ownerId: { type: String, required: true },
});

module.exports = mongoose.model('Book', bookSchema);
