# Node.js Express MongoDB Project Guide

A complete guide to setting up a Node.js application with Express and MongoDB.

## Project Overview

This project creates a simple REST API that performs CRUD operations on a user database using:
- Node.js as the runtime environment
- Express.js as the web framework
- MongoDB as the database
- Mongoose for MongoDB object modeling

## Prerequisites

- Node.js installed (v14 or higher recommended)
- npm (Node Package Manager)
- MongoDB account (local installation or MongoDB Atlas)
- Basic knowledge of JavaScript and RESTful APIs

## Step 1: Set Up Project Structure

1. Create a new project folder:
   ```bash
   mkdir express-mongodb-app
   cd express-mongodb-app
   ```

2. Initialize a new Node.js project:
   ```bash
   npm init -y
   ```

## Step 2: Install Dependencies

Install the necessary packages:

```bash
npm install express mongoose dotenv
npm install nodemon --save-dev
```

## Step 3: Create Configuration Files

1. Create a `.env` file in the root directory:
   ```
   PORT=3000
   MONGODB_URI=mongodb://localhost:27017/myapp
   ```
   
   If using MongoDB Atlas, your URI will look like:
   ```
   MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-url>/<database-name>?retryWrites=true&w=majority
   ```

2. Update `package.json` to include start scripts:
   ```json
   "scripts": {
     "start": "node app.js",
     "dev": "nodemon app.js"
   }
   ```

## Step 4: Set Up MongoDB Connection

1. **Local MongoDB Installation**:
   - Download and install MongoDB Community Edition
   - Start the MongoDB service

   OR

2. **MongoDB Atlas Setup**:
   - Create a MongoDB Atlas account
   - Create a new cluster
   - Set up database access (username/password)
   - Set up network access:
     - Add your current IP address OR
     - Allow access from anywhere (0.0.0.0/0) for development

## Step 5: Create Application Code

Create a file named `app.js` in the root directory:

```javascript
const express = require('express');
const mongoose = require('mongoose');
require('dotenv').config();

const app = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// MongoDB Connection
mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/myapp')
  .then(() => console.log('Connected to MongoDB'))
  .catch(err => console.error('Failed to connect to MongoDB', err));

// Define a simple schema and model
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true,
    unique: true
  },
  createdAt: {
    type: Date,
    default: Date.now
  }
});

const User = mongoose.model('User', userSchema);

// Routes
app.get('/', (req, res) => {
  res.send('Welcome to Express MongoDB API');
});

// Get all users
app.get('/api/users', async (req, res) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// Get single user
app.get('/api/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).json({ message: 'User not found' });
    res.json(user);
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// Create user
app.post('/api/users', async (req, res) => {
  const user = new User({
    name: req.body.name,
    email: req.body.email
  });

  try {
    const newUser = await user.save();
    res.status(201).json(newUser);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

// Update user
app.put('/api/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).json({ message: 'User not found' });
    
    if (req.body.name) user.name = req.body.name;
    if (req.body.email) user.email = req.body.email;
    
    const updatedUser = await user.save();
    res.json(updatedUser);
  } catch (err) {
    res.status(400).json({ message: err.message });
  }
});

// Delete user
app.delete('/api/users/:id', async (req, res) => {
  try {
    const user = await User.findById(req.params.id);
    if (!user) return res.status(404).json({ message: 'User not found' });
    
    await user.deleteOne();
    res.json({ message: 'User deleted' });
  } catch (err) {
    res.status(500).json({ message: err.message });
  }
});

// Start server
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Step 6: Run the Application

Start the development server:

```bash
npm run dev
```

Your server should start on port 3000 (or whatever port you specified in the .env file).

## Step 7: Using MongoDB Tools (Optional)

You can interact with your MongoDB database using various tools:

1. **MongoDB Compass**: A GUI tool for exploring and manipulating your data
2. **MongoDB Shell**: Command-line interface for MongoDB
3. **VS Code MongoDB Extension**: Interact with MongoDB directly from VS Code
   - Install the MongoDB extension
   - Connect to your database
   - Create a playground to run MongoDB commands

## Step 8: Testing Your API

Test your API endpoints using tools like Postman, curl, or any HTTP client:

1. **Create a User**:
   ```
   POST http://localhost:3000/api/users
   Content-Type: application/json

   {
     "name": "John Doe",
     "email": "john@example.com"
   }
   ```

2. **Get All Users**:
   ```
   GET http://localhost:3000/api/users
   ```

3. **Get a Single User**:
   ```
   GET http://localhost:3000/api/users/:id
   ```

4. **Update a User**:
   ```
   PUT http://localhost:3000/api/users/:id
   Content-Type: application/json

   {
     "name": "Jane Doe"
   }
   ```

5. **Delete a User**:
   ```
   DELETE http://localhost:3000/api/users/:id
   ```

## Troubleshooting Common Issues

1. **Connection Error**:
   - Verify MongoDB is running
   - Check if the connection string is correct
   - Ensure network access is properly configured in MongoDB Atlas

2. **Authentication Failed**:
   - Verify username and password in connection string
   - Check if the user has appropriate permissions

3. **IP Access Denied**:
   - Add your current IP to the whitelist in MongoDB Atlas

4. **Port Already in Use**:
   - Change the port in your .env file or close the application using that port

## Next Steps and Improvements

Once your basic application is working, consider these improvements:

1. Organize routes into separate files
2. Add input validation
3. Implement authentication and authorization
4. Create more complex relationships between data models
5. Add pagination for large datasets
6. Implement logging for debugging
7. Write tests for your API endpoints
8. Add documentation using tools like Swagger
