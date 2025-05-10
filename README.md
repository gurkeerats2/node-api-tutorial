Whether you're a frontend developer wanting to dip your toes into backend, or a beginner who wants to understand how APIs work — this guide is for you. We'll build a simple REST API using **Node.js + Express**, then test it with **Postman**.

---
## 📚 Table of Contents
1. [Introduction](#introduction)  
2. [Prerequisites](#prerequisites)  
3. [Project Setup](#project-setup)  
4. [Creating a Basic Server](#creating-a-basic-server)  
5. [Building the REST API (CRUD Users)](#building-the-rest-api-crud-users)  
6. [Testing with Postman](#testing-with-postman)  
7. [Optional Improvements](#optional-improvements)  
8. [Final Thoughts](#final-thoughts)

---
## 🔰 Introduction

In this tutorial, you'll learn how to:
- Create a REST API with Node.js and Express
- Add basic CRUD operations
- Test everything using Postman

You won’t need any database here — we’ll simulate one using an in-memory array. Perfect for learning the basics.

---
## 🧰 Prerequisites

Make sure you have the following installed:

- [Node.js](https://nodejs.org/)
- [Postman](https://www.postman.com/downloads/)
- A code editor like VS Code

---
## ⚙️ Project Setup

### Step 1: Initialize the project

```bash
mkdir node-api-tutorial
cd node-api-tutorial
npm init -y
```


### Step 2: Install dependencies

```bash
npm install express cors
npm install --save-dev nodemon
```


### Step 3: Create the entry file

📝 You can create this manually by:

Right-clicking in your file explorer and choosing New File (e.g., in VS Code).

![Create a new file in VS Code](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8dlvs5jznmmsodho3ht3.png)



---
## 🛠️ Creating a Basic Server

Paste this code in server.js:

```js
const express = require('express');
const cors = require('cors');

const app = express();
const port = 5000;

app.use(cors());
app.use(express.json());

app.get('/', (req, res) => {
  res.send('API is working!');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

Update your package.json to use nodemon:

```json
"scripts": {
  "start": "nodemon server.js"
}
```

Then run your server:

```bash
npm start
```
Visit http://localhost:5000 in your browser. You should see:

API is working!

---
## 🔄 Building the REST API (CRUD Users)
Add this below the root endpoint in server.js:

```js
let users = [
  { id: 1, name: 'Alice', email: 'alice@example.com' },
  { id: 2, name: 'Bob', email: 'bob@example.com' },
];

// GET all users
app.get('/users', (req, res) => {
  res.json(users);
});

// GET user by ID
app.get('/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');
  res.json(user);
});

// POST a new user
app.post('/users', (req, res) => {
  const newUser = {
    id: users.length + 1,
    name: req.body.name,
    email: req.body.email
  };
  users.push(newUser);
  res.status(201).json(newUser);
});

// PUT update user
app.put('/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');

  user.name = req.body.name;
  user.email = req.body.email;
  res.json(user);
});

// DELETE user
app.delete('/users/:id', (req, res) => {
  users = users.filter(u => u.id !== parseInt(req.params.id));
  res.status(204).send();
});
```
---
## 🧪 Testing with Postman
Open Postman and try out the following requests:

### ➕ POST  `` /users ``
Creates a new user
Method: POST
Endpoint: http://localhost:5000/users
Body (raw JSON):

```json
{
  "name": "Charlie",
  "email": "charlie@example.com"
}
```

![Postman screenshot for POST endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4un7wrxu8zwkxlrjpht3.png)


---
### 📥 GET  ``/users``
Returns all users.
Method: GET
Endpoint: http://localhost:5000/users

![Postman screenshot for GET users endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/h7pe3lo25znoxs5n5b5q.png)


---
### 📥 GET  ``/users/1``
Returns user with ID = 1.
Method: GET
Endpoint: http://localhost:5000/users/1

![Postman screenshot for GET user by ID endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jig3qvse40r0uurkm3rb.png)


---
### 🛠 PUT  ``/users/2``
Updates a user with ID = 2
Method: PUT
Endpoint: http://localhost:5000/users/2
Body:

```json
{
  "name": "Bobby",
  "email": "bobby@newmail.com"
}
```

![Postman screenshot for PUT endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/3h1tn4zyck70onu9c9hw.png)


---
#### ❌ DELETE  ``/users/1``
Deletes user with ID = 1.
Method: DELETE
Endpoint: http://localhost:5000/users/1

![Postman screenshot for Delete endpoint](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/mnvv3yoe6k9jkcssopjo.png)


If you like this tutorial, here's a blog to share with your folks
[How to Build a RESTful API in Node.js and Test it with Postman](https://dev.to/gurkeerats2/how-to-build-a-restful-api-in-nodejs-and-test-it-with-postman-378h)
