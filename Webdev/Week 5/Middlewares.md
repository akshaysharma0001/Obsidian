>In Express.js, **middleware** refers to functions that have access to the request object (`req`), response object (`res`), and the `next` function in the application's request-response cycle. Middleware functions can perform a variety of tasks, such as

1. Modifying the request or response objects.
2. Ending the request-response cycle.
3. Calling the next middleware function in the stack.

```javascript
app.use(function(req, res, next) {
    console.log("request received");
    next();
})
app.get("/sum", function(req, res) {
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});

/Output:request reviced
```

![[next.png]]

**Modifying request**
```javascript
app.use(function(req, res, next) {
    req.name = "harkirat"
    next();
})

app.get("/sum", function(req, res) {
    console.log(req.name);
    const a = parseInt(req.query.a);
    const b = parseInt(req.query.b);

    res.json({
        ans: a + b
    })
});
```

