npx create-react-app food-blog-app
cd food-blog-app
import React from 'react';
import './App.css';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <h1>Food Blog</h1>
      </header>
      <main>
        {/* Your blog post components will go here */}
      </main>
    </div>
  );
}

export default App;
mkdir food-blog-backend
cd food-blog-backend
npm init -y
npm install express mongoose
const express = require('express');
const mongoose = require('mongoose');
const app = express();
const PORT = process.env.PORT || 5000;

// Connect to MongoDB
mongoose.connect('mongodb://localhost/food-blog', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Define a schema and model for blog posts
const postSchema = new mongoose.Schema({
  title: String,
  content: String,
});

const Post = mongoose.model('Post', postSchema);

// API endpoint to fetch all blog posts
app.get('/api/posts', async (req, res) => {
  try {
    const posts = await Post.find();
    res.json(posts);
  } catch (error) {
    res.status(500).json({ error: 'Error fetching posts' });
  }
});

app.listen(PORT, () => {
  console.log(`Server is running on port ${PORT}`);
});
import React, { useState, useEffect } from 'react';
import './App.css';

function App() {
  const [posts, setPosts] = useState([]);

  useEffect(() => {
    fetch('/api/posts')
      .then((response) => response.json())
      .then((data) => setPosts(data))
      .catch((error) => console.error('Error fetching posts:', error));
  }, []);

  return (
    <div className="App">
      <header className="App-header">
        <h1>Food Blog</h1>
      </header>
      <main>
        <div className="post-list">
          {posts.map((post) => (
            <div key={post._id} className="post">
              <h2>{post.title}</h2>
              <p>{post.content}</p>
            </div>
          ))}
        </div>
      </main>
    </div>
  );
}

export default App;

