#Frontend Code
#React Component: BorrowButton.jsx
import React from 'react';
import axios from 'axios';

const BorrowButton = ({ bookId }) => {
  const handleBorrow = async () => {
    try {
      await axios.post('/api/transactions', { bookId, borrowerId: 'user123' });
      alert('Request to borrow sent!');
    } catch (error) {
      alert('Failed to send borrow request');
    }
  };

  return <button onClick={handleBorrow}>Borrow</button>;
};

export default BorrowButton;

