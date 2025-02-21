# bajajfrontend
import React, { useState } from "react";

const App = () => {
  const [input, setInput] = useState("{\"data\": []}");
  const [response, setResponse] = useState(null);
  const [error, setError] = useState(null);
  const [selectedFilters, setSelectedFilters] = useState([]);
  
  const handleSubmit = async () => {
    try {
      const parsedInput = JSON.parse(input);
      const res = await fetch("https://your-backend-url.herokuapp.com/bfhl", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(parsedInput),
      });
      const data = await res.json();
      setResponse(data);
      setError(null);
    } catch (err) {
      setError("Invalid JSON or server error");
      setResponse(null);
    }
  };
  
  const handleFilterChange = (e) => {
    const value = e.target.value;
    setSelectedFilters(
      selectedFilters.includes(value)
        ? selectedFilters.filter((f) => f !== value)
        : [...selectedFilters, value]
    );
  };

  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>BFHL Frontend</h1>
      <textarea
        rows="5"
        cols="50"
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder='Enter JSON (e.g., {"data": ["A", "1", "B"]})'
      />
      <br />
      <button onClick={handleSubmit}>Submit</button>
      {error && <p style={{ color: "red" }}>{error}</p>}
      {response && (
        <>
          <h2>Response</h2>
          <select multiple onChange={handleFilterChange}>
            <option value="numbers">Numbers</option>
            <option value="alphabets">Alphabets</option>
            <option value="highest_alphabet">Highest Alphabet</option>
          </select>
          <div>
            {selectedFilters.includes("numbers") && <p>Numbers: {JSON.stringify(response.numbers)}</p>}
            {selectedFilters.includes("alphabets") && <p>Alphabets: {JSON.stringify(response.alphabets)}</p>}
            {selectedFilters.includes("highest_alphabet") && <p>Highest Alphabet: {JSON.stringify(response.highest_alphabet)}</p>}
          </div>
        </>
      )}
    </div>
  );
};

export default App;
