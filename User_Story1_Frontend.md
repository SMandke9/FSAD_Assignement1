#Frontend Code
#React Component: AddBookForm.jsx
import React, { useState } from 'react';
import axios from 'axios';

const AddBookForm = () => {
  const [bookData, setBookData] = useState({
    title: '',
    author: '',
    genre: '',
    condition: '',
  });

  const handleChange = (e) => {
    setBookData({ ...bookData, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post('/api/books', {
        ...bookData,
        ownerId: 'user123', // Replace with actual user ID from session/context
      });
      alert('Book added successfully');
    } catch (error) {
      alert('Failed to add book');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="title" placeholder="Title" onChange={handleChange} required />
      <input name="author" placeholder="Author" onChange={handleChange} required />
      <select name="genre" onChange={handleChange} required>
        <option value="">Select Genre</option>
        <option value="Classic">Classic</option>
        <option value="Fiction">Fiction</option>
      </select>
      <input name="condition" placeholder="Condition" onChange={handleChange} required />
      <button type="submit">Add Book</button>
    </form>
  );
};

export default AddBookForm;