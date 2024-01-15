# BHARATHINTERN-TASK03
#Using HTML
<!DOCTYPE html>
<html>
<head>
  <title>Money Tracker App</title>
  <link rel="stylesheet" type="text/css" href="styles.css">
</head>
<body>
  <header>
    <h1>Money Tracker</h1>
  </header>
  
  <main>
    <section id="expenses">
      <h2>Expenses</h2>
      <!-- Expense tracking form -->
      <form id="expense-form">
        <label for="expense-name">Expense Name:</label>
        <input type="text" id="expense-name" name="expense-name" required>
        
        <label for="expense-amount">Expense Amount:</label>
        <input type="number" id="expense-amount" name="expense-amount" required>
        
        <button type="submit">Add Expense</button>
      </form>
      
      <!-- Expense list -->
      <ul id="expense-list">
        <!-- Expense items will be dynamically added here -->
      </ul>
    </section>
    
    <section id="income">
      <h2>Income</h2>
      <!-- Income tracking form -->
      <form id="income-form">
        <label for="income-name">Income Name:</label>
        <input type="text" id="income-name" name="income-name" required>
        
        <label for="income-amount">Income Amount:</label>
        <input type="number" id="income-amount" name="income-amount" required>
        
        <button type="submit">Add Income</button>
      </form>
      
      <!-- Income list -->
      <ul id="income-list">
        <!-- Income items will be dynamically added here -->
      </ul>
    </section>
  </main>
  
  <script src="app.js"></script>
</body>
</html>

#Using CSS
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

header {
  background-color: #333;
  color: #fff;
  padding: 20px;
}

h1 {
  margin: 0;
}

section {
  margin-bottom: 20px;
}

form {
  margin-bottom: 10px;
}

label {
  display: block;
  margin-bottom: 5px;
}

input[type="text"],
input[type="number"] {
  width: 200px;
  padding: 5px;
}

button {
  padding: 5px 10px;
  background-color: #333;
  color: #fff;
  border: none;
  cursor: pointer;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  margin-bottom: 5px;
}

#Using NDE.JS In MANGDB
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');

const app = express();
const port = 3000;

// Connect to MongoDB
mongoose.connect('mongodb://localhost/money_tracker', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => {
    console.log('Connected to MongoDB');
  })
  .catch((error) => {
    console.error('Error connecting to MongoDB:', error);
  });

// Expense schema and model
const expenseSchema = new mongoose.Schema({
  name: String,
  amount: Number
});

const Expense = mongoose.model('Expense', expenseSchema);

// Income schema and model
const incomeSchema = new mongoose.Schema({
  name: String,
  amount: Number
});

const Income = mongoose.model('Income', incomeSchema);

// Middleware
app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

// Routes
app.get('/expenses', (req, res) => {
  Expense.find()
    .then((expenses) => {
      res.json(expenses);
    })
    .catch((error) => {
      console.error('Error retrieving expenses:', error);
      res.status(500).json({ error: 'Error retrieving expenses' });
    });
});

app.post('/expenses', (req, res) => {
  const { name, amount } = req.body;

  const expense = new Expense({
    name,
    amount
  });

  expense.save()
    .then(() => {
      res.sendStatus(201);
    })
    .catch((error) => {
      console.error('Error saving expense:', error);
      res.status(500).json({ error: 'Error saving expense' });
    });
});

app.get('/income', (req, res) => {
  Income.find()
    .then((income) => {
      res.json(income);
    })
    .catch((error) => {
      console.error('Error retrieving income:', error);
      res.status(500).json({ error: 'Error retrieving income' });
    });
});

app.post('/income', (req, res) => {
  const { name, amount } = req.body;

  const income = new Income({
    name,
    amount
  });

  income.save()
    .then(() => {
      res.sendStatus(201);
    })
    .catch((error) => {
      console.error('Error saving income:', error);
      res.status(500).json({ error: 'Error saving income' });
    });
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
