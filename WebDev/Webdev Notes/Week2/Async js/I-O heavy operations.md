**I/O (Input/Output) heavy operations** refer to tasks in a computer program that involve a lot of data transfer between the program and external systems or devices. These operations usually require waiting for data to be read from or written to sources like disks, networks, databases, or other external devices, which can be time-consuming compared to in-memory computations.

### Examples of I/O Heavy Operations:

1. Reading a file
2. Starting a clock
3. HTTP Requests

  

We’re going to introduce imports/requires next. A `require` statement lets you import code/functions export from another file/module.

  

Let’s try to write code to do an `I/O` heavy operation -

1. Open repl.it
2. Create a file in there (a.txt) with some text inside
3. Write the code to read a file `synchronously`

```jsx
const fs = require("fs");

const contents = fs.readFileSync("a.txt", "utf-8");
console.log(contents);
```

4. Create another file (b.txt)
5. Write the code to read the other file `synchronously`

```jsx
const fs = require("fs");

const contents = fs.readFileSync("a.txt", "utf-8");
console.log(contents);

const contents2 = fs.readFileSync("b.txt", "utf-8");
console.log(contents2);
```

  

What is wrong in this code above?