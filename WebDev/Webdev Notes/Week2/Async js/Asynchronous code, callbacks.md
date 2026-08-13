Let’s look at the code to read from a file `asynchronously`. Here, we pass in a `function` as an `argument`. This function is called a `callback` since the function gets `called` `back` when the file is read

![[Screenshot_2024-08-10_at_6.43.49_PM.png]]

  

```jsx
const fs = require("fs");

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});
```

## setTimeout

setTimeout is another asynchronous function that executes a certain code after some time

```jsx
function run() {
	console.log("I will run after 1s");
}

setTimeout(run, 1000);
console.log("I will run immedietely");
```