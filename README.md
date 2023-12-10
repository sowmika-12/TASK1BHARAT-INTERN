# TASK1BHARAT-INTERN
plain text           /project-root
|-- public
|   |-- styles.css
|-- views
|   |-- index.html
|-- server.js
|-- package.json
npm init -y
npm install express mongoose body-parser

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="/styles.css">
  <title>User Registration</title>
</head>
<body>
  <div class="container">
    <h2>User Registration</h2>
    <form id="registrationForm" action="/register" method="post">
      <label for="username">Username:</label>
      <input type="text" id="username" name="username" required>

      <label for="email">Email:</label>
      <input type="email" id="email" name="email" required>

      <label for="password">Password:</label>
      <input type="password" id="password" name="password" required>

      <button type="submit">Register</button>
    </form>
  </div>
</body>
</html>

body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.container {
  background-color: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

form {
  display: flex;
  flex-direction: column;
}

label {
  margin-bottom: 8px;
}

input {
  padding: 8px;
  margin-bottom: 16px;
}

button {
  padding: 10px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}


const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const PORT = process.env.PORT || 3000;

// Connect to MongoDB (replace 'your-mongodb-uri' with your actual MongoDB connection string)
mongoose.connect('mongodb://localhost:27017/your-database-name', { useNewUrlParser: true, useUnifiedTopology: true });

// Create a mongoose schema and model for user
const userSchema = new mongoose.Schema({
  username: String,
  email: String,
  password: String
});

const User = mongoose.model('User', userSchema);

// Middleware to parse the request body
app.use(bodyParser.urlencoded({ extended: true }));

// Serve static files from the 'public' directory
app.use(express.static('public'));

// Serve the registration form
app.get('/', (req, res) => {
  res.sendFile(__dirname + '/views/index.html');
});

// Handle user registration
app.post('/register', async (req, res) => {
  const { username, email, password } = req.body;

  try {
    // Save the user to the database
    const newUser = new User({ username, email, password });
    await newUser.save();

    res.send('Registration successful!');
  } catch (error) {
    res.status(500).send('Error registering user.');
  }
});

// Start the server
app.listen(PORT, () => {
  console.log(Server is running on http://localhost:${PORT});
});
