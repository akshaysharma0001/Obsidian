![[password hashing.png]]
## Why should you hash passwords?

Password hashing is a technique used to securely store passwords in a way that makes them difficult to recover or misuse. Instead of storing the actual password, you store a hashed version of it.

## salt

A popular approach to hashing passwords involves using a hashing algorithm that incorporates a salt—a random value added to the password before hashing. This prevents attackers from using precomputed tables (rainbow tables) to crack passwords.

## bcrypt

**Bcrypt**: It is a cryptographic hashing algorithm designed for securely hashing passwords. Developed by Niels Provos and David Mazières in 1999, bcrypt incorporates a salt and is designed to be computationally expensive, making brute-force attacks more difficult.

## Base code

We’re starting from yesterday’s code - [https://github.com/100xdevs-cohort-3/week-7-mongo](https://github.com/100xdevs-cohort-3/week-7-mongo)

## Adding password encryption

- Install the `bcrypt` library - [https://www.npmjs.com/package/bcrypt](https://www.npmjs.com/package/bcrypt)
    
    ![Screenshot 2024-09-15 at 6.43.20 PM.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/085e8ad8-528e-47d7-8922-a23dc4016453/7f4fba53-1604-4648-b38c-a2e96c6255a1/Screenshot_2024-09-15_at_6.43.20_PM.png)
    
- Update the `/signup` endpoint
    

```jsx
app.post("/signup", async function(req, res) {
    const email = req.body.email;
    const password = req.body.password;
    const name = req.body.name;

    const hasedPassword = await bcrypt.hash(password, 10);

    await UserModel.create({
        email: email,
        password: hasedPassword,
        name: name
    });
    
    res.json({
        message: "You are signed up"
    })
});

```

- Password format
- ![[passwordformat.png]]

![[passwordformat detail.png]]
    
    So, putting it all together:
    
    - **`$2b$`**: Version of bcrypt.
    - **`10$`**: Cost factor (saltRounds).
    - **`wyemvgfpjkEzg2dzuRyM9e`**: Salt value (base64 encoded).
    - **`LrQZnT69X/tj0KW/zM6TZhnrvT.TCne`**: Hashed password (base64 encoded).
- Update the `signin` function

```javascript
app.post("/signin", async function(req, res) {
    const email = req.body.email;
    const password = req.body.password;

    const user = await UserModel.findOne({
        email: email,
    });

    const passwordMatch = bcrypt.compare(password, user.password);
    if (user && passwordMatch) {
        const token = jwt.sign({
            id: user._id.toString()
        }, JWT_SECRET);

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

