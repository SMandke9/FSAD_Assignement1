#Frontend Code
#React Component: SearchBooks.jsx
import React, { useState } from 'react';
import axios from 'axios';

const SearchBooks = () => {
  const [query, setQuery] = useState({ genre: '', title: '' });
  const [books, setBooks] = useState([]);

  const handleSearch = async () => {
    try {
      const response = await axios.get('/api/books', { params: query });
      setBooks(response.data);
    } catch (error) {
      alert('No books found');
    }
  };

  return (
    <div>
      <input
        placeholder="Title"
        onChange={(e) => setQuery({ ...query, title: e.target.value })}
      />
      <select onChange={(e) => setQuery({ ...query, genre: e.target.value })}>
        <option value="">Select Genre</option>
        <option value="Classic">Classic</option>
        <option value="Fiction">Fiction</option>
      </select>
      <button onClick={handleSearch}>Search</button>
      <ul>
        {books.map((book) => (
          <li key={book._id}>{book.title} by {book.author}</li>
        ))}
      </ul>
    </div>
  );
};

export default SearchBooks;
