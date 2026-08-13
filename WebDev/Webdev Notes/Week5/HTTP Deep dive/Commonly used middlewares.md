Through your `journey of writing express servers` , you’ll find some commonly available (on npm) middlewares that you might want to use

### 1. express.json

The `express.json()` middleware is a built-in middleware function in Express.js used to parse incoming request bodies that are formatted as JSON. This middleware is essential for handling JSON payloads sent by clients in POST or PUT requests.

```jsx
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

Try converting the `calculator` assignment to use `POST` endpoints. Check if it works with/without the `express.json` middleware

Express uses `bodyParser` under the hood - https://github.com/expressjs/express/blob/master/lib/express.js#L78C16-L78C26