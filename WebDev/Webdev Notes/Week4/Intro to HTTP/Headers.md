HTTP headers are key-value pairs included in HTTP requests and responses that provide `metadata` about the message.

  

## Why not use body?

Even though you can use body for everything, it is a good idea to use `headers` for sending data that isn’t directly related with the `application logic`.

For example, if you want to create a new TODO, you will send the TODO payload in the body

```jsx
{
   description: "Go to gym"
}
```

But the `Authorization` information in the `headers`

```jsx
Authorization: harkirat
```