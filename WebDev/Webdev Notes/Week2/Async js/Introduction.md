## Shoutouts

Web based wallets ($50 each)

https://x.com/kairveee/status/1822263287079174271

![[Screenshot_2024-08-10_at_7.35.07_PM.png]]

https://x.com/DweetParikh/status/1822249456664019332

![[Screenshot_2024-08-10_at_6.56.45_PM.png]]

- Reports Platform ($100 for creator) - https://report-100xdevs.vercel.app/

## Goal of todays class -

1. I/O tasks
2. Callbacks
3. Functional arguments
4. Async vs Sync code
5. Event loops, callback queues, JS

### Goal of tomorrows class

1. Async await, Promises
2. Practising async JS

  

**Hopefully, by the end of the class, you are able to understand the following code -**

Functional arguments

```jsx
function sum(a, b) {
  return a + b;
}

function multiply(a, b) {
  return a * b;
}

function subtract(a, b) {
  return a - b;
}

function divide(a, b) {
  return a / b;
}

function doOperation(a, b, op) {
  return op(a, b)
}

console.log(doOperation(1, 2, sum))
```

Callbacks

```jsx
const fs = require("fs");

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});
```