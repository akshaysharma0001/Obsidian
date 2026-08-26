>Route-specific middleware in Express.js refers to middleware functions that are applied only to specific routes or route groups, rather than being used globally across the entire application

```javascript
const express = require('express');
const app = express();

// Middleware function
function logRequest(req, res, next) {
  console.log(`Request made to: ${req.url}`);
  next();
}

// Apply middleware to a specific route
app.get('/special', logRequest, (req, res) => {
  res.send('This route uses route-specific middleware!');
});

app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

# Assignments on middleware
Try these out yourself.
1. Create a middleware function that logs each incoming request’s HTTP method, URL, and timestamp to the console
2. Create a middleware that counts total number of requests sent to a server. Also create an endpoint that exposes it

```javascript
// Create a middleware that counts total number of requests sent to a server. Also create an endpoint that exposes it

  

let countreq=0;
import express from 'express'
const app=new express()

function logRequest(req,res,next){
    console.log("Request method : "+req.method)
    console.log("url : "+req.url)
    let date=new Date().toLocaleTimeString()
    console.log(date)
    countreq++;
    next()
}

app.get('/special',logRequest,(req,res)=>{
    res.send("Middleware request sent")
})
app.get('/stats',logRequest,(req,res)=>{
    res.send("Number of request sent "+countreq)
})
app.listen(3000,()=>{

    console.log("server running on port 3000")

})
```