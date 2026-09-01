- Through your journey of writing express servers you’ll find some commonly available (on npm) middlewares that you might want to use

**1.express.json()**

The *express.json()* middleware is a built-in middleware function in Express.js used to parse incoming request bodies that are formatted as JSON. This middleware is essential for handling JSON payloads sent by clients in POST or PUT requests.

```javascript
const express = require('express');
const app = express();

// Use express.json() middleware to parse JSON bodies
app.use(express.json());

// Define a POST route to handle JSON data
app.post('/data', (req, res) => {
  // Access the parsed JSON data from req.body
  const data = req.body;
  console.log('Received data:', data);

  // Send a response
  res.send('Data received');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

- If we are creating our own custom middle ware like count number of request we can call it everytime when we are calling any method Or we can app.use(logRequest)

```javascript
// Create a middleware that counts total number of requests sent to a server. Also create an endpoint that exposes it

let count=0;
import express from 'express'
import bodyParser from 'body-parser'
const app=new express

app.use(express.json())
//app.use(bodyParser.json())

app.use(logRequest)

function logRequest(req,res,next){

    console.log("Request method : "+req.method)

    console.log("url : "+req.url)

    let date=new Date().toLocaleTimeString()

    console.log(date)

    count++

    req.count=count

    next()

}

  

app.get('/special',(req,res)=>{
    res.send("Middleware request sent")
})  
app.get('/stats',(req,res)=>{
    res.send("Number of request sent "+req.count)
})
app.listen(3000,()=>{

    console.log("server running on port 3000")

})
```