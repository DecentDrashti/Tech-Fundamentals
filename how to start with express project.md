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
