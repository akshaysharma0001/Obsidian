Lets now create a `todo application` with the data being `persisted` in the database.

- Initialise a new Node.js project

```jsx
npm init -y
```

- Install dependencies

```jsx
npm install express mongoose
```

- Create the skeleton for 4 routes
    
    - POST /signup
    - POST /login
    - POST /todo (authenticated)
    - GET /todos (authenticated)
- Solution
    
    ```jsx
    const express = require("express");
    
    const app = express();
    app.use(express.json());
    
    app.post("/signup", function(req, res) {
        
    });
    
    app.post("/signin", function(req, res) {
    
    });
    
    app.post("/todo", function(req, res) {
    
    });
    
    app.get("/todos", function(req, res) {
    
    });
    
    app.listen(3000);
    ```
    
- Initialize the schema of your app in a new file (db.js)
    
- Easy schema
    
    ```jsx
    const mongoose = require("mongoose");
    
    const Schema = mongoose.Schema;
    const ObjectId = Schema.ObjectId;
    
    const User = new Schema({
      name: String,
      email: String,
      password: String
    });
    
    const Todo = new Schema({
        userId: ObjectId,
        title: String,
        done: Boolean
    });
    
    const UserModel = mongoose.model('users', User);
    const TodoModel = mongoose.model('todos', Todo);
    
    module.exports = {
        UserModel,
        TodoModel
    }
    ```
    
- Hard schema
    
    ```jsx
    const mongoose = require("mongoose");
    
    const Schema = mongoose.Schema;
    const ObjectId = Schema.ObjectId;
    
    const User = new Schema({
      name: String,
      email: {type: String, unique: true},
      password: String
    });
    
    const Todo = new Schema({
        userId: ObjectId,
        title: String,
        done: Boolean
    });
    
    const UserModel = mongoose.model('users', User);
    const TodoModel = mongoose.model('todos', Todo);
    
    module.exports = {
        UserModel,
        TodoModel
    }
    ```
    
- Import the model in `index.js`
    

```jsx
const { UserModel, TodoModel } = require("./db");
```

- Implement the `/signup` endpoint

```jsx
app.post("/signup", async function(req, res) {
    const email = req.body.email;
    const password = req.body.password;
    const name = req.body.name;

    await UserModel.create({
        email: email,
        password: password,
        name: name
    });
    
    res.json({
        message: "You are signed up"
    })
});
```

- Implement the `/signin` endpoint (need to install jsonwebtoken library)

```jsx
const JWT_SECRET = "s3cret";

app.post("/signin", async function(req, res) {
    const email = req.body.email;
    const password = req.body.password;

    const response = await UserModel.findOne({
        email: email,
        password: password,
    });

    if (response) {
        const token = jwt.sign({
            id: response._id.toString()
        })

        res.json({
            token
        })
    } else {
        res.status(403).json({
            message: "Incorrect creds"
        })
    }
});
```

- Implement the `auth` middleware (in a new file auth.js)

```jsx
const jwt = require("jsonwebtoken");
const JWT_SECRET = "s3cret";

function auth(req, res, next) {
    const token = req.headers.authorization;

    const response = jwt.verify(token, JWT_SECRET);

    if (response) {
        req.userId = token.userId;
        next();
    } else {
        res.status(403).json({
            message: "Incorrect creds"
        })
    }
}

module.exports = {
    auth,
    JWT_SECRET
}
```

- Implement the `POST` todo endpoint

```jsx
const { auth, JWT_SECRET } = require("./auth");

```

- Connect to your DB at the top of index.js

# Improvements

1. Password is not hashed
2. A single crash (duplicate email) crashes the whole app
3. Add more endpoints (mark todo as done)
4. Add timestamp at which todo was created/the time it needs to be done by
5. Relationships in Mongo
6. Add validations to ensure email and password are correct format