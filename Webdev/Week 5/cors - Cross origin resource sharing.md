Cross-Origin Resource Sharing (CORS) is a security feature implemented by web browsers that controls how resources on a web server can be requested from another domain. It's a crucial mechanism for managing *cross-origin* requests and ensuring secure interactions between *different origins* on the web.

```javascript
app.use(cors({
origin:"localhost:3000"
}))
```
**CORS from browser**

![[CORS.png]]

**Same request from Postman**

![[cors from postman.png]]

### Realworld usage of CORS
**Create HTTP server**
```javascript
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

**Create index.html file**

```javascript
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

> You dont need cors if the frontend and backend are on the same domain

```javascript
const express=require("express")
const cors=require("cors")  
const app=new express()

app.use(express.json())
app.use(cors()) 
app.post('/add',(req,res)=>{
    let a =Number(req.body.a)
    let b=Number(req.body.b)
    res.send((a+b))
})
  
app.get('/',(req,res)=>{

    res.sendFile(__dirname + "/index.htm")

})

app.listen(3000)
```

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/axios/1.7.6/axios.min.js"></script>
</head>
<body>
  <div id="posts">
    <input type="text" id="a">
    <input type="text" id="b">
    <button onclick="sendRequest()">add </button>
  </div>
<p></p>
  <script>
    async function sendRequest() {
      const res = await axios.post("http://localhost:3000/add",{"a":parseInt(document.getElementById("a").value),"b":parseInt(document.getElementById("b").value)});
      document.querySelector('p').innerHTML=res.data
    } 
 </script>
</body> 
</html>
```