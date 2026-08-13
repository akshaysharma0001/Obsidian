**Cross-Origin Resource Sharing (CORS)** is a security feature implemented by web browsers that controls how resources on a web server can be requested from another domain. It's a crucial mechanism for managing `cross-origin` requests and ensuring secure interactions between `different origins` on the web.

  

### Cross origin request from the browser

![[Screenshot_2024-08-31_at_7.36.42_PM.png]]

### Same request from Postman

![[Screenshot_2024-08-31_at_7.37.08_PM.png]]

  

## Real world example

Create an HTTP Server

- Create an HTTP Server

```jsx
const express = require("express");

const app = express();

app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

app.listen(3000);
```

- Create an index.html file (public/index.html)

```jsx
<!DOCTYPE html>
<html>

<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.7.6/axios.min.js"></script>
</head>

<body>
  <div id="posts"></div>
  <script>
    async function sendRequest() {
      const res = await axios.get("http://localhost:3000/sum?a=1&b=2");
    }

    sendRequest();
  </script>
</body>

</html>
```

- Serve the HTML File on a `different port`

```jsx
cd public
npx serve
```

![[Screenshot_2024-08-31_at_7.40.59_PM.png]]

You will notice the `cross origin request` fails

- Add cors as a dependency

```jsx
npm i cors
```

- Use the `cors` middleware

```jsx
const express = require("express");
const cors = require("cors");
const app = express();
app.use(cors());

app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

app.listen(3000);
```

![[Screenshot_2024-08-31_at_7.42.39_PM.png]]

  

## You dont need `cors` if the frontend and backend are on the same domain

- Try serving the frontend on the same domain

```jsx
const express = require("express");
const app = express();

app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

app.get("/", function(req, res) {
    res.sendFile(__dirname + "/public/index.html");
});

app.listen(3000);
```

- Go to `[localhost:3000](http://localhost:3000)` , notice that the underlying request doesnt fail with `cors` (even though we don’t have the cors middleware)
    
	![[Screenshot_2024-08-31_at_7.45.08_PM.png]]