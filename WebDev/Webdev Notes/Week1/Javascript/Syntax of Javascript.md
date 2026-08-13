### 1. **Variables**

Variables are used to store data. In JavaScript, you declare variables using `var`, `let`, or `const`.

```jsx
let name = "John";     // Variable that can be reassigned
const age = 30;        // Constant variable that cannot be reassigned
var isStudent = true;  // Older way to declare variables, function-scoped
```

Assignment

Create a variable for each of the following: your favorite color, your height in centimeters, and whether you like pizza. Use appropriate variable declarations (`let`, `const`, or `var`). Try logging it using `console.log`

### 2. Data types

```jsx
let number = 42;             // Number
let string = "Hello World";  // String
let isActive = false;        // Boolean
let numbers = [1, 2, 3];     // Array
```

### 3. **Operators**

```jsx
let sum = 10 + 5;          // Arithmetic operator
let isEqual = (10 === 10); // Comparison operator
let isTrue = (true && false); // Logical operator
```

### 4. **Functions**

```jsx
// Function declaration
function greet(name) {
    return "Hello, " + name;
}

// Function call
let message = greet("John"); // "Hello, John"
```

Assignment #1

Write a function `sum` that finds the sum of two numbers.  
Side quest - Try passing in a string instead of a number and see what happens?

Assignment #2

Write a function called `canVote` that returns true or false if the `age` of a user is > 18

### 5. If/Else

```jsx
if (age >= 18) {
    console.log("You are an adult.");
} else {
    console.log("You are a minor.");
}
```

Assignment

Write an if/else statement that checks if a number is even or odd. If it's even, print "The number is even." Otherwise, print "The number is odd."

### 6. Loops

```jsx
// For loop
for (let i = 0; i < 5; i++) {
    console.log(i); // Outputs 0 to 4
}

// While loop
let j = 0;
while (j < 5) {
    console.log(j); // Outputs 0 to 4
    j++;
}
```

  

Assignment

Write a function called sum that finds the `sum` from 1 to a number