const express = require('express');
const { MongoClient } = require('mongodb');
require('dotenv').config();

const app = express();
app.use(express.json());

const mongoUri = process.env.MONGO_URI || 'mongodb://usuario:password@host:puerto/nombre_base';
let db;

MongoClient.connect(mongoUri, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(client => {
    db = client.db();
    console.log('Connected to MongoDB');
  })
  .catch(error => console.error('Error connecting to MongoDB:', error));

app.post('/query', async (req, res) => {
  try {
    const { collection, query } = req.body;
    if (!collection) {
      return res.status(400).json({ error: 'Collection name is required' });
    }

    const result = await db.collection(collection).find(query || {}).toArray();
    res.json(result);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
