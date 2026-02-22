# crud for 3 tables posts, product,user
## install
    npm init -y
    npm install express mysql2 cors dotenv

## project structure
    project/
    │
    ├── server.js
    ├── db.js
    ├── routes/
    │     ├── users.js
    │     ├── posts.js
    │     └── products.js
    └── .env

## db.js
    require("dotenv").config();
    const mysql = require("mysql2");
    
    const db = mysql.createConnection({
      host: process.env.DB_HOST,
      user: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_NAME,
    });
    
    db.connect((err) => {
      if (err) console.error("DB error:", err);
      else console.log("MySQL Connected ✅");
    });
    
    module.exports = db;

## server.js
    const express = require("express");
    const cors = require("cors");
    
    const usersRoute = require("./routes/users");
    const postsRoute = require("./routes/posts");
    const productsRoute = require("./routes/products");
    
    const app = express();
    app.use(cors());
    app.use(express.json());
    
    app.use("/users", usersRoute);
    app.use("/posts", postsRoute);
    app.use("/products", productsRoute);
    
    app.listen(3000, () => {
      console.log("Server running on port 3000 🚀");
    });

## routes/user.js
    const express = require("express");
    const db = require("../db");
    
    const router = express.Router();
    
    // CREATE
    router.post("/", (req, res) => {
      const { name, email } = req.body;
      db.query(
        "INSERT INTO users (name, email) VALUES (?, ?)",
        [name, email],
        (err, result) => {
          if (err) return res.status(500).json(err);
          res.json({ message: "User created", id: result.insertId });
        }
      );
    });
    
    // READ ALL
    router.get("/", (req, res) => {
      db.query("SELECT * FROM users", (err, results) => {
        if (err) return res.status(500).json(err);
        res.json(results);
      });
    });
    
    // READ ONE
    router.get("/:id", (req, res) => {
      db.query(
        "SELECT * FROM users WHERE id = ?",
        [req.params.id],
        (err, results) => {
          if (err) return res.status(500).json(err);
          if (results.length === 0)
            return res.status(404).json({ message: "Not found" });
          res.json(results[0]);
        }
      );
    });
    
    // UPDATE
    router.put("/:id", (req, res) => {
      const { name, email } = req.body;
      db.query(
        "UPDATE users SET name=?, email=? WHERE id=?",
        [name, email, req.params.id],
        (err) => {
          if (err) return res.status(500).json(err);
          res.json({ message: "Updated" });
        }
      );
    });
    
    // DELETE
    router.delete("/:id", (req, res) => {
      db.query(
        "DELETE FROM users WHERE id=?",
        [req.params.id],
        (err) => {
          if (err) return res.status(500).json(err);
          res.json({ message: "Deleted" });
        }
      );
    });
    
    module.exports = router;
    
    ### same for post and products
