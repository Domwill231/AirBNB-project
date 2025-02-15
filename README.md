# AirBNB-project
The following analysis is to determine which neighborhoods and property sizes are most attractive
for vacation rentals and how much money did they generate. 
Through the data cleaning process, we were able to determine that Chelsea ,East Harlem, East Village, 
Harlem, Hell's Kitchen, Lower East Side, Midtown, Upper East Side, 
Upper West Side, and West Village were the most desired neighborhoods. 
Also, we gathered that 1 bedrooms were the most desired in all of the neighborhoods except for Harlem. 
Studios were more desired in Harlem. 
Lastly, we were able to see how much each property would generate over a 12 month span. 
Investing in 1 bedroom properties in these neighborhoods will continue to generate profit. 
Properties that causes profit loss can be sold to the general public.

airbnb-clone/
├── backend/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── server.js
│   └── config/
├── frontend/
│   ├── public/
│   ├── src/
│   ├── package.json
│   └── ...
└── package.json
mkdir airbnb-clone
cd airbnb-clone
mkdir backend
cd backend
npm init -y
npm install express mongoose dotenv
const express = require('express');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
dotenv.config();
const app = express();
const PORT = process.env.PORT || 5000;
// Middleware
app.use(express.json());
// MongoDB Connection
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('MongoDB connected'))
    .catch(err => console.log(err));
// Routes
app.use('/api/listings', require('./routes/listings'));
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT}`);
});
const mongoose = require('mongoose');
const ListingSchema = new mongoose.Schema({
    title: {
        type: String,
        required: true
    },
    description: {
        type: String,
        required: true
    },
    price: {
        type: Number,
        required: true
    },
    location: {
        type: String,
        required: true
    },
    image: {
        type: String,
        required: true
    }
});
module.exports = mongoose.model('Listing', ListingSchema);
const Listing = require('../models/Listing');
// @desc    Get all listings
// @route   GET /api/listings
// @access  Public
exports.getListings = async (req, res) => {
    try {
        const listings = await Listing.find();
        res.status(200).json(listings);
    } catch (err) {
        res.status(500).json({ message: 'Server Error' });
    }
};
// @desc    Create a listing
// @route   POST /api/listings
// @access  Public
exports.createListing = async (req, res) => {
    try {
        const newListing = new Listing(req.body);
        const listing = await newListing.save();
        res.status(201).json(listing);
    } catch (err) {
        res.status(500).json({ message: 'Server Error' });
    }
};
const express = require('express');
const router = express.Router();
const { getListings, createListing } = require('../controllers/listingController');
router.route('/')
    .get(getListings)
    .post(createListing);
module.exports = router;
npx create-react-app frontend
cd frontend
import React from 'react';
const Listing = ({ listing }) => {
    return (
        <div className="listing">
            <img src={listing.image} alt={listing.title} />
            <h2>{listing.title}</h2>
            <p>{listing.description}</p>
            <p>Location: {listing.location}</p>
            <p>Price: ${listing.price}</p>
        </div>
    );
};
export default Listing;
import React, { useEffect, useState } from 'react';
import Listing from './Listing';
const Listings = () => {
    const [listings, setListings] = useState([]);
    useEffect(() => {
        const fetchListings = async () => {
            const res = await fetch('/api/listings');
            const data = await res.json();
            setListings(data);
        };
        fetchListings();
    }, []);
    return (
        <div className="listings">
            {listings.map(listing => (
                <Listing key={listing._id} listing={listing} />
            ))}
        </div>
    );
};
export default Listings;
import React from 'react';
import Listings from './components/Listings';
const App = () => {
    return (
        <div className="App">
            <h1>Airbnb Clone</h1>
            <Listings />
        </div>
    );
};
export default App;
{
    "name": "frontend",
    "version": "0.1.0",
    "private": true,
    "dependencies": {
        "react": "^17.0.1",
        "react-dom": "^17.0.1",
        "react-scripts": "4.0.0"
    },
    "scripts": {
        "start": "react-scripts start",
        "build": "react-scripts build",
        "test": "react-scripts test",
        "eject": "react-scripts eject"
    },
    "proxy": "http://localhost:5000"
}
MONGO_URI=mongodb://localhost:27017/airbnb-clone
PORT=5000
cd backend
node server.js
cd ../frontend
npm start
