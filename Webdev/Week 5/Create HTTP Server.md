It should have 4 routes
1. [http://localhost:3000/multiply?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)
2. [http://localhost:3000/add?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)
3. [http://localhost:3000/divide?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)
4. [http://localhost:3000/subtract?a=1&b=2](http://localhost:3000/multiply?a=1&b=2)

```javascript
import express from 'express'
const app=new express()

app.get('/add',(req,res)=>{
    let a =Number(req.query.a)
    let b=Number(req.query.b)
    res.send((a+b))
})

app.get('/mult',(req,res)=>{
    let a =Number(req.query.a)
    let b=Number(req.query.b)
    res.send((a*b))
})

app.get('/sub',(req,res)=>{
    let a =Number(req.query.a)
    let b=Number(req.query.b)
    res.send((a-b))
})  

app.get('/div',(req,res)=>{
    let a =Number(req.query.a)
    let b=Number(req.query.b)
    res.send((a/b))
})

//dynamic route handler
app.get('/add/:a/:b',(req,res)=>{
    let a =Number(req.params.a)
    let b=Number(req.params.b)
    res.send((a+b))

})
app.listen(3001)
```