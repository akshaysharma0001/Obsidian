TypeScript offers two ways to work with types:
1. **Explicit Typing**: You explicitly declare the type of a variable
2. **Type Inference**: TypeScript automatically determines the type based on the assigned value

## When TypeScript Can't Infer Types

- While TypeScript's type inference is powerful, there are cases where it can't determine the correct type.

- In these situations, TypeScript falls back to the `any` type, which disables type checking.

```typescript
// 1. JSON.parse returns 'any' because the structure isn't known at compile time  
const data = JSON.parse('{ "name": "Alice", "age": 30 }');  
  
// 2. Variables declared without initialization  
let something;  // Type is 'any'  
something = 'hello';  
something = 42;  // No error
```


## Type: any
- The `any` type is the most flexible type in TypeScript.
-  It essentially tells the compiler to skip type checking for a particular variable.
- While this can be useful in certain situations, it should be used sparingly as it bypasses TypeScript's type safety features.