```javascript
import express from 'express'
const app=new express()

app.get('/',(req,res)=>{
    res.send('Hello world')
})

app.listen(3000,()=>{
    console.log("http://localhost:3000/")
})
```

**Program to calculate natural sum of numbers**

```javascript
import express from 'express'
const app=new express()

app.get('/',(req,res)=>{
    let n=req.query.n
    res.send(naturalsum(n))
})

app.listen(3000,()=>{
    console.log("http://localhost:3000/")
})

function naturalsum(n){
    let sum=0
    for(let i =1;i<=n;i++){
        sum+=i
    }
    return sum
}

//http:localhost:3000/?n=55
```

**Program to read a file and return **

```javascript
import express from 'express'
import fs from 'fs'
const app=express()
app.get('/:filename',(req,res)=>{
    let file=req.params.filename
    fs.readFile(file,"utf-8",(err,data)=>{
        if(err){
            res.status(404).send("File not found")
        }
        else
        {
            res.send(data)
        }
    })
})
app.listen(3000)

//http:localhost:3000/a.txt
```