React and Node.js Assignment



This assignment aims to create a full-stack application using React for the frontend and Node.js with Express and PostgreSQL for the backend. The assignment requirements are as follows:

(1) Backend (Node.js with Express and PostgreSQL):
Create a PostgreSQL database table with the following columns: sno, customer_name, age, phone, location, created_at.
Populate the table with 50 dummy records containing random data.
Implement an Express API endpoint to fetch the customer data from the database.

>> set up Node js project:
mkdir backend
cd backend
npm init -y

>> Install required Dependies:
mkdir backend
cd backend
npm init -y

>>Create a server.js file for your Express server:

// server.js

const express = require('express');
const { Pool } = require('pg');

const app = express();
const port = 5000;

// PostgreSQL connection configuration
const pool = new Pool({
  user: 'your_username',
  host: 'localhost',
  database: 'your_database_name',
  password: 'your_password',
  port: 5432,
});

// Create table and insert 50 dummy records
pool.query(`
  CREATE TABLE IF NOT EXISTS customers (
    sno SERIAL PRIMARY KEY,
    customer_name VARCHAR(255),
    age INT,
    phone VARCHAR(20),
    location VARCHAR(255),
    created_at TIMESTAMP DEFAULT NOW()
  )
`)
  .then(() => {
    console.log('Table created successfully');
    // Insert 50 dummy records
    for (let i = 1; i <= 50; i++) {
      pool.query(`
        INSERT INTO customers (customer_name, age, phone, location)
        VALUES ('Customer ${i}', ${Math.floor(Math.random() * 50) + 20}, '1234567890', 'Location ${i}')
      `);
    }
  })
  .catch(err => console.error('Error executing query', err));

// Define routes
app.get('/api/customers', async (req, res) => {
  try {
    const result = await pool.query('SELECT * FROM customers');
    res.json(result.rows);
  } catch (err) {
    console.error('Error executing query', err);
    res.status(500).json({ error: 'Internal Server Error' });
  }
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});

(2) Frontend (React):
Create a React application to display the customer data in a table format.
Implement pagination to display 20 records per page.
Add a search feature to filter data by customer name or location.
Split the created_at timestamp into separate "date" and "time" columns.
Provide an option to sort the data by "date" or "time" columns.

Frontend (React)

>>Set up your React project:
#bash
npx create-react-app frontend
cd frontend
# Create a src/components directory and add CustomerTable.js:
#Javascript

// src/components/CustomerTable.js

import React, { useEffect, useState } from 'react';

const CustomerTable = () => {
  const [customers, setCustomers] = useState([]);

  useEffect(() => {
    fetch('/api/customers')
      .then(res => res.json())
      .then(data => setCustomers(data))
      .catch(err => console.error('Error fetching data', err));
  }, []);

  return (
    <div>
      <h2>Customer List</h2>
      <table>
        <thead>
          <tr>
            <th>S.No</th>
            <th>Customer Name</th>
            <th>Age</th>
            <th>Phone</th>
            <th>Location</th>
            <th>Created Date</th>
            <th>Created Time</th>
          </tr>
        </thead>
        <tbody>
          {customers.map(customer => (
            <tr key={customer.sno}>
              <td>{customer.sno}</td>
              <td>{customer.customer_name}</td>
              <td>{customer.age}</td>
              <td>{customer.phone}</td>
              <td>{customer.location}</td>
              <td>{new Date(customer.created_at).toLocaleDateString()}</td>
              <td>{new Date(customer.created_at).toLocaleTimeString()}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default CustomerTable;

>> Update src/App.js to use CustomerTable component:
#javascript

// src/App.js

import React from 'react';
import CustomerTable from './components/CustomerTable';

function App() {
  return (
    <div className="App">
      <CustomerTable />
    </div>
  );
}

export default App;

Backend Implementation
The backend is implemented using Node.js with Express and PostgreSQL.
The server.js file sets up an Express server and establishes a connection to the PostgreSQL database.
It creates a customers table in the database and populates it with 50 dummy records.
An API endpoint /api/customers is defined to fetch the customer data from the database.


Frontend Implementation
The frontend is implemented using React.
The CustomerTable.js component fetches the customer data from the backend API and displays it in a table format.
Pagination, search functionality, and sorting options are implemented within the component.

Getting Started
To run the application locally:

Clone this repository to your local machine.
Navigate to the backend directory and run npm install to install backend dependencies.
Run npm start to start the backend server.
Navigate to the frontend directory and run npm install to install frontend dependencies.
Run npm start to start the frontend development server.
Open your browser and visit http://localhost:3000 to view the application.

Technologies Used

Node.js
Express
PostgreSQL
React
HTML
CSS

Author
Narisipalli Kusuma.






