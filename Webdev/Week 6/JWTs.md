JWT (JSON Web Token)
There is a problem with using `stateful` tokens.
## Stateful (normal token)
By stateful here, we mean that we need to store these tokens in a variable right now (and eventually in a database).

## Problem with token
The problem is that we need to `send a request to the database` every time the user wants to hit an `authenticated endpoint`

## Solution
==JWTs==

# JWTs

JWTs, or JSON Web Tokens, are a compact and self-contained way to represent information between two parties. They are commonly used for authentication and information exchange in web applications.

**JWTs are Stateless**: JWTs contain all the information needed to authenticate a request, so the server doesn’t need to store session data. All the `data` is stored in the token itself.

# Replace token logic with jwt

Lets change the token logic that we had to use jwts

- Add the `jsonwebtoken` library as a dependency - [https://www.npmjs.com/package/jsonwebtoken](https://www.npmjs.com/package/jsonwebtoken)
    
    ```jsx
    npm install jsonwebtoken
    ```
    
- Get rid of our `generateToken` function
    
    ```jsx
    function generateToken() {
        let options = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z', 'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S', 'T', 'U', 'V', 'W', 'X', 'Y', 'Z', '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
    
        let token = "";
        for (let i = 0; i < 32; i++) {
            // use a simple function here
            token += options[Math.floor(Math.random() * options.length)];
        }
        return token;
    }
    ```
    
- Create a `JWT_SECRET` variable
    
    ```jsx
    const JWT_SECRET = "USER_APP";
    ```
    
- Create a jwt for the user instead of generating a token
    
    ```jsx
    app.post("/signin", (req, res) => {
        const username = req.body.username;
        const password = req.body.password;
    
        const user = users.find(user => user.username === username && user.password === password);
    
        if (user) {
            const token = jwt.sign({
                username: user.username
            }, JWT_SECRET);
    
            user.token = token;
            res.send({
                token
            })
            console.log(users);
        } else {
            res.status(403).send({
                message: "Invalid username or password"
            })
        }
    });
    ```
    

<aside>  
💡

Notice we put the `username` inside the token. The `jwt` holds your state.  
You no longer need to store the `token` in the global `users` variable

</aside>

- In the `/me` endpoint, use `jwt.verify` to verify the token
    
    ```jsx
    
    app.get("/me", (req, res) => {
        const token = req.headers.authorization;
        const userDetails = jwt.verify(token, JWT_SECRET);
    
        const username =  userDetails.username;
        const user = users.find(user => user.username === username);
    
        if (user) {
            res.send({
                username: user.username
            })
        } else {
            res.status(401).send({
                message: "Unauthorized"
            })
        }
    })
    ```

# JWTs can be DECODED by anyone

JWTs can be decoded by anyone. They can be `verified` by only the server that issued them.

Ref - [https://jwt.io/](https://jwt.io/)

Try creating a jwt and decoding it on the website. You’ll notice it does decode. But that is fine

## Comparision to a cheque.

If you ever sign a `cheque`, you can show it to everyone and everyone can see that you are transferring $20 to a friend. But only the BANK `needs to verify` before debiting the users account.

Doesnt matter if everyone sees the cheque, they cant do anything with this information.

But the `bank` can `verify` the signature and do whatever the end users asked to do

JWTs can be coded by everyone

	JWTs can be verified by only the person who issued them (using the JWT secret)