To `initialize` a Node.js project locally,

- Run the following command -

```jsx
npm init -y
```

- Exploring package.json

![[Screenshot_2024-08-24_at_6.22.45_PM.png]]

- Writing some code

```jsx
let firstName = "Harkirat Singh"
console.log(firstName)
```

- Run the code

```jsx
node index.js
```

- Add a `script` in `package.json`

```jsx
  "scripts": {
    "start": "node index.js"
  },
```

- Run `npm run start`