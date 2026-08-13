It should have 4 routes

1. http://localhost:3000/multiply?a=1&b=2
2. [http://localhost:3000/add?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)
3. [http://localhost:3000/divide?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)
4. [http://localhost:3000/subtract?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)

Inputs given at the end after `?` are known as query parameters (usually used in GET requests)

The way to get them in an HTTP route is by extracting them from the `req` argument (req.query.a , req.query.b)

## Steps to follow

- Initialize node project

```jsx
npm init -y
```

- Install dependencies

```jsx
npm install express
```

- Create an empty index.js

```jsx
touch index.js
```

- Write the code to create 4 endpoints

```jsx
const express = require("express");

const app = express();

app.get("/sum", function(req, res) {

});

app.get("/multiply", function(req, res) {
    
});

app.get("/divide", function(req, res) {
    

});

app.get("/subtract", function(req, res) {

});

app.listen(3000);
```

- Fill in the handler body

```jsx
const express = require("express");

const app = express();

app.get("/sum", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;

    res.json({
        ans: a + b
    })
});

app.get("/multiply", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;
    res.json({
        ans: a * b
    })
});

app.get("/divide", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;
    res.json({
        ans: a / b
    })

});

app.get("/subtract", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;
    res.json({
        ans: a - b
    })
});

app.listen(3000);
```

- Test it in the browser
    
	![[Screenshot_2024-08-31_at_7.13.02_PM.png]]
    

What do you think is wrong here?

- Correct the solution

```jsx
const express = require("express");

const app = express();

app.get("/sum", function(req, res) {
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

app.get("/multiply", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;
    res.json({
        ans: a * b
    })
});

app.get("/divide", function(req, res) {
    const a = req.query.a;
    const b = req.query.b;
    res.json({
        ans: a / b
    })

});

app.get("/subtract", function(req, res) {
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);
    res.json({
        ans: a - b
    })
});

app.listen(3000);
```