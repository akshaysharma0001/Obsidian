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

**Todo list program using HTTP and express**

```javascript
import express from 'express'
import fs from 'fs'
const app=new express()

let data=JSON.parse(fs.readFileSync("todo.json","utf-8"))
app.use(express.json())
app.get('/',(req,res)=>{
    res.send(printtodo())
    })

app.post('/',(req,res)=>{
    let task=req.body
    data[data.length]=task
    res.send("<h3>Task added Succesfully</h3>")
    fs.writeFileSync("todo.json",JSON.stringify(data))
})

app.delete('/',(req,res)=>{
    let i=JSON.stringify(req.body)
    data.splice(data[i.num],1)
    res.send("<h3>Delete succesfully</h3>")  
})
app.listen(3000,()=>{
    console.log("http://localhost:3000/")
})

function printtodo(){
    let list=""
    for(let i=0;i<data.length;i++){
        list+="<h3>"+(i+1)+". "+data[i].task+"</h3>"
    }
    return list
}
```