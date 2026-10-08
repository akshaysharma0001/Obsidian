## What is TypeScript?

TypeScript is a syntactic superset of JavaScript which adds **static typing**.

This basically means that TypeScript adds syntax on top of JavaScript, allowing developers to add **types**.

## Why should I use TypeScript?

JavaScript is a loosely typed language.

It can be difficult to understand what types of data are being passed around in JavaScript.

In JavaScript, function parameters and variables don't have any information!

So developers need to look at documentation, or guess based on the implementation.

TypeScript allows specifying the types of data being passed around within the code, and has the ability to report errors when the types don't match.

For example, TypeScript will report an error when passing a string into a function that expects a number.


# First program
```typescript
const greet = (name: string): string => {

return `Hello ${name}`;

};

const x: string = greet("akshay");


console.log(x);
```

*Compile* tsc hello.ts -> node run hello.js