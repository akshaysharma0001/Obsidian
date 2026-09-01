- Can you try creating a middleware called auth that verifies if a user is logged in and ends the request early if the user isn’t logged in?

```javascript
const express = require('express')
const jwt= require('jsonwebtoken')
const JWT_SECRET="JWT_SECRET"
  
const app=new express()
  
let users=[]
function auth(req,res,next){
    const token=req.headers.token
    if(token){
        jwt.verify(token,JWT_SECRET,(err,decoded)=>{
            if(err){
                res.status(401).send({"message":"Unauthorized"})
            }
            else{
                req.user=decoded
                next()
            }
        })
    }
    else{
        next()
    }
}
  
app.use(express.json())
app.use(auth)
  
app.post('/signup',(req,res)=>{
    let username=req.body.username
    let password=req.body.password
    users.push({
        "username":username,
        "password":password
    })
    res.send({
        "message":"User added succesfully"
    })
    console.log(users)
})
  
app.post('/signin',(req,res)=>{
    let username=req.body.username
    let password=req.body.password

    let user=users.find(u=>u.username===username &&  u.password==password)
    if(user){
        const token=jwt.sign({"username":username},JWT_SECRET)
        res.send({
            "token":token
        })
        console.log({"token":token})
    }
    else{
        res.send({"message":"Invalid username or password"})
    }
})
  
app.get('/me',auth,(req,res)=>{
    const user=req.user
    res.send({"username":user.username})
})
  
app.get('/',(req,res)=>{
    res.sendFile(__dirname+"/frontend-login/index.htm")
})

app.listen(3000,()=>{
    console.log("http://localhost:3000/")
})
```
