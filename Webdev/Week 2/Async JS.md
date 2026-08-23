### Synchronous code
- Synchronous code is executed line by line, in the order it's written. Each operation waits for the previous one to complete before moving on to the next one.

```
- function sum(n) {
	let ans = 0;
	for (let i = 1; i <= n; i++) {
		ans = ans + i
	}
	return ans;
}

const ans1 = sum(100);
console.log(ans1);
const ans2 = sum(1000);
console.log(ans2);
const ans3 = sum(10000);
console.log(ans3);

```

# I/O heavy operations
**I/O (Input/Output) heavy operations** refer to tasks in a computer program that involve a lot of data transfer between the program and external systems or devices. These operations usually require waiting for data to be read from or written to sources like disks, networks, databases, or other external devices, which can be time-consuming compared to in-memory computations.

#### Examples of I/O Heavy Operations:
1. Reading a file
2. Starting a clock
3. HTTP Requests

- `require` statement lets you import code/functions export from another file/module

```
const fs = require("fs");
const contents = fs.readFileSync("a.txt", "utf-8");
console.log(contents);

```  

- *CPU-bound tasks* are operations that are limited by the speed and power of the CPU. These tasks require significant computation and processing power, meaning that the performance bottleneck is the CPU itself. ex-loops
#### Async readFile function

```
const fs = require("fs");

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});

fs.readFile("b.txt", "utf-8", function (err, contents) {
  console.log(contents);
});

fs.readFile("a.txt", "utf-8", function (err, contents) {
  console.log(contents);
});

```

## Functional arguments
- Function passed as a function