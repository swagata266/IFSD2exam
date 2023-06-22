const express = require('express');
const mongoose = require('mongoose');

// Connect to MongoDB
mongoose
  .connect('mongodb+srv://Swagate:ankush123@cluster0.fjrkc4l.mongodb.net/?retryWrites=true&w=majority', {
    useNewUrlParser: true,
    useUnifiedTopology: true
  })
  .then(() => {
    console.log('Connected to MongoDB');
  })
  .catch((error) => {
    console.error('Error connecting to MongoDB:', error);
  });

// Define the shop schema
const shopSchema = new mongoose.Schema({
  name: String,
  rent: Number
});

// Create the Shop model
const Shop = mongoose.model('Shop', shopSchema);

// Define the shopping complex schema
const shoppingComplexSchema = new mongoose.Schema({
  shops: [shopSchema]
});

// Create the ShoppingComplex model
const ShoppingComplex = mongoose.model('ShoppingComplex', shoppingComplexSchema);

// Create an Express app
const app = express();

// Add middleware for parsing JSON
app.use(express.json());

// Create an instance of ShoppingComplex
const complex = new ShoppingComplex();

// Define a route for adding a shop
app.post('/shops', (req, res) => {
  const { name, rent } = req.body;

  // Create a new shop
  const shop = new Shop({ name, rent });

  // Add the shop to the shopping complex
  complex.shops.push(shop);

  // Save the shopping complex to MongoDB
  complex
    .save()
    .then(() => {
      res.status(201).json({ message: 'Shop added successfully' });
    })
    .catch((error) => {
      res.status(500).json({ error: 'Failed to add the shop' });
    });
});

// Define a route for calculating the total shop rent
app.get('/totalRent', (req, res) => {
  // Calculate the total shop rent
  const totalRent = complex.shops.reduce((sum, shop) => sum + shop.rent, 0);
  res.json({ totalRent });
});

// Start the server
const port = 3000;
app.listen(port, () => {
  console.log(`Server is running on http://localhost:${port}`);
});
