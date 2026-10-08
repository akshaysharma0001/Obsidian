

- The most basic types in TypeScript are called **primitives**.
- These types form the building blocks of more complex types in your applications.
- TypeScript includes all JavaScript primitives plus additional type features.

# Types
1. Boolean
2. Number
3. String
4. BigInt
5. Symbol

### Symbol
- In TypeScript, a **`symbol`** is a primitive data type (introduced in ES2015) used to create **completely unique and immutable identifiers**. Even if two symbols are created with the exact same description, they are entirely distinct from one another

- The primary use case for symbols is as unique keys for object properties. Because they are unique, they guarantee that your properties will never conflict with other property keys (e.g., from third-party libraries)

#### 1. Using Symbols as Object Keys
- The primary use case for symbols is as unique keys for object properties. Because they are unique, they guarantee that your properties will never conflict with other property keys (e.g., from third-party libraries)

