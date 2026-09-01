![[indexjs.png]]- Initialize a new Node.js project
- Add Express, jsonwebtoken, mongoose to it as a dependency
- Create index.js
- Add route skeleton for user login, signup, purchase a course, sees all courses, sees the purchased courses course
- Add routes for admin login, admin signup, create a course, delete a course, add course content.
- Define the schema for User, Admin, Course, Purchase
- Add a database (mongodb), use dotenv to store the database connection string
- Add middlewares for user and admin auth
- Complete the routes for user login, signup, purchase a course, see course (Extra points - Use express routing to better structure your routes)
- Create the frontend

### Good to haves

- Use cookies instead of JWT for auth
- Add a rate limiting middleware
- Frontend in ejs (low pri)
- Frontend in React

# Routing 
Routing in Express.js determines how an application's endpoints respond to client requests based on a specific URI (or path) and an HTTP request method

- Approach 1 
- course.js
		![[coursejs.png]]
		
	- Index.js![[indexjs.png]]


![[databaSchema.png]]

**Index.js file**

```javascript
const express=require('express')
const mongoose=require('mongoose')
require('dotenv').config()


const {userRouter}=require('./routes/user.js')
const {courseRouter}=require('./routes/course.js')
const {adminRouter}=require('./routes/admin.js')

const app=new express()

app.use(express.json())

app.post('/',(req,res)=>{

})

app.use('/user',userRouter)

app.use('/course',courseRouter)

app.use('/admin',adminRouter)

async function main(){
    await mongoose.connect(process.env.MONGO_CON)
    console.log("DB connected")
    app.listen(3000,()=>{
    console.log("http://localhost:3000")
})

}

try{
    main()
}catch(e){
    console.log("Cant connect to db")
}


```

**db.js file**

```javascript
const mongoose=require('mongoose')
const Schema=mongoose.Schema

const userSchema=new Schema({

    email:{type:String,unique:true},
    password:String,
    firstName:String,
    lastName:String
})

const adminSchema=new Schema({

    email:String,
    password:String,
    firstName:String,
    lastName:String
})

const courseSchema=new Schema({
    
    title:String,
    description:String,
    price:Number,
    imageUrl:String,
    creatorId:{type:mongoose.Types.ObjectId,ref:'admins'}

})

const purchaseSchema=new Schema({
    
    courseId:{type:mongoose.Types.ObjectId,ref:'courses'},
    userId:{type:mongoose.Types.ObjectId,ref:'users'}
})

const adminModel=mongoose.model('admins',adminSchema)
const courseModel=mongoose.model('courses',courseSchema)
const userModel=mongoose.model('users',userSchema)
const purchaseModel=mongoose.model('purchases',purchaseSchema)

module.exports={
    adminModel:adminModel,
    courseModel:courseModel,
    userModel:userModel,
    purchaseModel:purchaseModel
}
```

**middleware.js**

```javascript
const jwt=require('jsonwebtoken')

function authUSER(req,res,next){
    const token=req.headers.token
    let verify=jwt.verify(token,process.env.JWT_USER)

    if(verify){
        req.userId=verify.id
        console.log(req.userId)
        next()
    }else{
        res.json({"message":"unauthorized access"})
    }
}

function authADMIN(req,res,next){
    const token=req.headers.token
    let verify=jwt.verify(token,process.env.JWT_ADMIN)

    if(verify){
        req.userId=verify.id
        console.log(req.userId)
        next()
    }else{
        res.json({"message":"unauthorized access"})
    }
}

function logger(req,res,next){
    console.log(req.method,+" " +req.url)
    next()
}

module.exports={
    authUSER:authUSER,
    authADMIN:authADMIN,
    logger:logger
}
```

**admin.js (admin routes)**

```javascript
const express=require('express')

const {Router}=require('express')

const adminRouter=Router()

const {adminModel, courseModel}=require('../db')

const zod=require('zod')

const bcrypt=require('bcrypt')

const jwt=require('jsonwebtoken')

const mongoose=require('mongoose')

require('dotenv').config()

const {authADMIN,logger}=require('../middleware/middlewares')


const app=express()

app.use(express.json())

adminRouter.post('/signup',logger,async (req,res)=>{
    const email=req.body.email
    const password=req.body.password
    const firstName=req.body.firstName
    const lastName=req.body.lastName

    const verifyFormat=zod.object({
        "email":zod.string().email().min(3).max(30),
        "password":zod.string().min(6).max(30),
        "lastName":zod.string().min(3).max(30),
        "firstName":zod.string().min(3).max(30)
    })

    const verifyFormatSuccess=verifyFormat.safeParse(req.body)

    if(!verifyFormatSuccess.success){
        console.log(verifyFormatSuccess.error.issues.map(issue=>issue.message))

        return res.json({"message":verifyFormatSuccess.error.issues.map(issue=>issue.message)})

    }
    else{
          const duplicate=await adminModel.findOne({email:email})
    
        if(duplicate){
            return res.json({"message":"user already exist"})
        }
        else{
            const hashedPassword=await bcrypt.hash(password,5)
            await adminModel.create({
            email:email,
            password:hashedPassword,
            firstName:firstName,
            lastName:lastName
        })
    
        res.json({"message":"signed up succesfully"})
    
        }

    
    }
    
})

adminRouter.post('/login',logger,async (req,res)=>{

    const email=req.body.email
    const password=req.body.password

    const response=await adminModel.findOne({email:email})

    if(!response){
        return res.json({"message":"user doesnt exist"})
    }
    else{
        const verifyPassword=await bcrypt.compare(password,response.password)

    if(verifyPassword){
        const token=jwt.sign({"id":response._id.toString()},process.env.JWT_SECRET)

        res.json({"token":token})
    }
    else{
        res.json({
            "message":"Incorrect password"
        })
    }
    }

})


adminRouter.post('/course',logger,authADMIN,async(req,res)=>{
    const title=req.body.title
    const description=req.body.description
    const price=req.body.price
    const imageUrl=req.body.imageUrl
    console.log(req.userId)
    const creatorId=new mongoose.Types.ObjectId(req.userId)

    const course=await courseModel.create({
        title:title,
        description:description,
        price:price,
        imageUrl:imageUrl,
        creatorId:creatorId
    })

    res.json({message:"course created succesfully",
        course:course
    })
})


adminRouter.put('/course',logger,authADMIN,async (req,res)=>{
    const adminId=body.userId

    const {title,description,price,imageUrl}=req.body

    const course=await courseModel.updateOne({
        _id:courseId,
        createrId:adminId
    },
    {
        title:title,
        description:description,
        imageUrl:imageUrl,
        price:price
        
    })

    res.json({"message":"course updated successfully",
        courseId:courseId,
    })
})

adminRouter.get('/course/bulk',logger,authADMIN,async(req,res)=>{

    const adminId=req.userId
    
    const courses=await courseModel.find({
        creatorId:adminId
    })
    res.json({message:courses})
    console.log(courses)
})

module.exports={
    adminRouter:adminRouter
}
```


**user.js (user routes)**
```javascript
const mongoose=require('mongoose')
const Schema=mongoose.Schema

const userSchema=new Schema({

    email:{type:String,unique:true},
    password:String,
    firstName:String,
    lastName:String
})

const adminSchema=new Schema({

    email:String,
    password:String,
    firstName:String,
    lastName:String
})

const courseSchema=new Schema({
    
    title:String,
    description:String,
    price:Number,
    imageUrl:String,
    creatorId:{type:mongoose.Types.ObjectId,ref:'admins'}

})

const purchaseSchema=new Schema({
    
    courseId:{type:mongoose.Types.ObjectId,ref:'courses'},
    userId:{type:mongoose.Types.ObjectId,ref:'users'}
})

const adminModel=mongoose.model('admins',adminSchema)
const courseModel=mongoose.model('courses',courseSchema)
const userModel=mongoose.model('users',userSchema)
const purchaseModel=mongoose.model('purchases',purchaseSchema)

module.exports={
    adminModel:adminModel,
    courseModel:courseModel,
    userModel:userModel,
    purchaseModel:purchaseModel
}
```


**course.js (course routes)**

```javascript
const {Router} =require('express')

const courseRouter=Router()

const {authUSER,logger}=require('../middleware/middlewares')
const { courseModel, purchaseModel } = require('../db')
const { default: mongoose } = require('mongoose')

courseRouter.get('/preview',logger,async (req,res)=>{
    const courses=await courseModel.find({})
    res.json({courses})
})

courseRouter.post('/purchase',logger,authUSER,async(req,res)=>{
    const userId=new mongoose.Types.ObjectId(req.userId)
    const courseId=new mongoose.Types.ObjectId(req.body.courseId)

    const purchase=await purchaseModel.create({
        courseId:courseId,
        userId:userId
    })

    res.json({"message":"course purchased succesfully"})

})

courseRouter.get('/purchases',logger,authUSER,async(req,res)=>{
    const userId=new mongoose.Types.ObjectId(req.userId)
    const purchase=await purchaseModel.find({userId:userId})
    res.json(purchase)
})

module.exports={
    courseRouter:courseRouter
}
```