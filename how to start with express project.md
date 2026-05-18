step 1 check the node install or install it from the official website 
https://nodejs.org/en/download
step 2 npm init -y which will develop package.json
step 3 install express
step 4 create server.js 
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Backend Running Successfully");
});

app.listen(5000, () => {
    console.log("Server started on port 5000");
});
step 5 run node sever.js and check in postman 
step 6 npm install mongoose dotenv for mongodb connection and all
| Package  | Purpose                           |
| -------- | --------------------------------- |
| mongoose | Connect Node.js to MongoDB        |
| dotenv   | Read secret variables from `.env` |
step 7 then create and env file 
step 8 creating account in mongodb atlas create cluster for free after creating click on it and set up username and password and remember it then connect with driver copy the link and paste it in .env file 
step 9 create db.js inside the config folder(if you wanna create a precise industry level folder mechanism or creating a heavy project)
