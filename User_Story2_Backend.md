#Backend Code
#Route: routes/transactions.js
const express = require('express');
const router = express.Router();
const { borrowBook } = require('../controllers/transactionController');

// POST /api/transactions
router.post('/', borrowBook);

module.exports = router;

#Controller: controllers/transactionController.js
const Transaction = require('../models/Transaction');

exports.borrowBook = async (req, res) => {
  try {
    const { bookId, borrowerId } = req.body;

    if (!bookId || !borrowerId) {
      return res.status(400).json({ error: 'Book ID and Borrower ID are required' });
    }

    const newTransaction = new Transaction({
      bookId,
      borrowerId,
      status: 'pending',
    });
    const savedTransaction = await newTransaction.save();

    res.status(201).json(savedTransaction);
  } catch (error) {
    res.status(500).json({ error: 'Failed to borrow book' });
  }
};

#Model: models/Transaction.js
const mongoose = require('mongoose');

const transactionSchema = new mongoose.Schema({
  bookId: { type: String, required: true },
  borrowerId: { type: String, required: true },
  status: { type: String, default: 'pending' },
});

module.exports = mongoose.model('Transaction', transactionSchema);