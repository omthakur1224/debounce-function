import { useState, useEffect } from 'react';

function useDebouncedSearch(callback, delay) {
  const [searchTerm, setSearchTerm] = useState('');
  const [debouncedSearchTerm, setDebouncedSearchTerm] = useState('');
  
  // Update the debounced search term whenever the input value changes
  useEffect(() => {
    const debounceTimeout = setTimeout(() => {
      setDebouncedSearchTerm(searchTerm);
    }, delay);

    return () => {
      clearTimeout(debounceTimeout);
    };
  }, [searchTerm, delay]);

  // Call the callback function when the debounced search term changes
  useEffect(() => {
    if (debouncedSearchTerm !== '') {
      callback(debouncedSearchTerm);
    }
  }, [debouncedSearchTerm, callback]);

  // Function to handle input changes and update the search term
  const handleSearchChange = (event) => {
    setSearchTerm(event.target.value);
  };

  return {
    searchTerm,
    handleSearchChange,
  };
}

export default useDebouncedSearch;

//how to use in components
import React from 'react';
import useDebouncedSearch from './useDebouncedSearch'; // Import your custom hook

function SearchComponent() {
  const { searchTerm, handleSearchChange } = useDebouncedSearch((query) => {
    // Perform your search logic here with the 'query' parameter
    console.log('Searching for:', query);
    // You can update your search results state or make an API request here
  }, 500); // Set your desired debounce delay (in milliseconds)

  return (
    <div>
      <input
        type="text"
        placeholder="Search..."
        value={searchTerm}
        onChange={handleSearchChange}
      />
      {/* Display your search results here */}
    </div>
  );
}

export default SearchComponent;
