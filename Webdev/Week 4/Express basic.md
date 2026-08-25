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
```

