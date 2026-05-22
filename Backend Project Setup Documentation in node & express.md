# Node.js + Express + MongoDB Backend Setup Guide

## Step 1: Check Node.js Installation

Check whether Node.js is installed on your system.

If not installed, download and install it from:

https://nodejs.org/en/download

---

## Step 2: Initialize Node Project

Run:

```bash
npm init -y
```

This will generate:

```text
package.json
```

---

## Step 3: Install Express

Run:

```bash
npm install express
```

---

## Step 4: Create `server.js`

Create a file named:

```text
server.js
```

Add the following code:

```javascript
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Backend Running Successfully");
});

app.listen(5000, () => {
    console.log("Server started on port 5000");
});
```

---

## Step 5: Run Server and Test

Run:

```bash
node server.js
```

Check the API using:

- Postman
- Browser

---

## Step 6: Install MongoDB Dependencies

Run:

```bash
npm install mongoose dotenv
```

### Package Purpose

| Package | Purpose |
|----------|----------|
| mongoose | Connect Node.js to MongoDB |
| dotenv | Read secret variables from `.env` |

---

## Step 7: Create `.env` File

Create a file named:

```text
.env
```

This file will store sensitive information such as:

- MongoDB Connection String
- Secret Keys
- Environment Variables

---

## Step 8: Create MongoDB Atlas Account and Cluster

1. Create an account in MongoDB Atlas.
2. Create a free cluster.
3. Open the cluster.
4. Set up:
   - Username
   - Password
5. Remember both credentials.
6. Click:

```text
Connect
```

7. Select:

```text
Drivers
```

8. Copy the MongoDB connection string.
9. Paste it inside the `.env` file.

---

## Step 9: Create `db.js`

Create:

```text
config/db.js
```

Use this if you want:

- Precise industry-level folder structure
- Heavy project architecture
- Better project organization

---

## Step 10: Create Application Layers

### Create Model (Schema)

Create your MongoDB schema using Mongoose.

---

### Create Controller

Create methods for:

- Insert
- Update
- Delete
- Fetch

inside the controller.

---

### Create Routes

Define routes inside the route file.

---

### Register Routes in `server.js`

Import route files and add them to:

```text
server.js
```

---

## Step 11: Login Authentication

### 1. Install bcrypt for Password Hashing

Run:

```bash
npm install bcryptjs
```

---

### 2. Create Registration Controller

Create a registration controller and use:

- `genSalt()`
- `hash()`

for password hashing before saving passwords to the database.
