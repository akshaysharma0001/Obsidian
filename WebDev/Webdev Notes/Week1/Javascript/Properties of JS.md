Every language comes with it’s unique set of features.

Javascript has the following -

# 1. Interpreted

JavaScript is an interpreted language, meaning it's executed line-by-line at runtime by the JavaScript engine in the browser or server environment, rather than being compiled into machine code beforehand.

![[Screenshot_2024-08-04_at_6.04.48_PM.png]]

  

**Upsides -**

1. There is one less step to do before running your code

**Downsides -**

1. Performance Overhead:
2. More prone to runtime errors

# 2. Dynamically Typed

Variables in JavaScript are not bound to a specific data type. Types are determined at runtime and can change as the program executes

### C++ Code (won’t compile)

```cpp
\#include <iostream>

int main() { 
  int a = 1;
  a = "hello";
  a = true;
}
```

### JS Code (will compile)

```jsx

var a = 1;
a = "harkirat";
a = true;

console.log(a)
```

  

# 3. Single threaded

JavaScript executes code in a single-threaded environment, meaning it processes one task at a time. We will dive deeper into this next week.

![[Screenshot_2024-08-04_at_6.12.41_PM.png]]

![[Screenshot_2024-08-04_at_6.13.11_PM.png]]

# 4. Garbage collected

JavaScript automatically manages memory allocation and deallocation through garbage collection, which helps prevent memory leaks by automatically reclaiming memory used by objects no longer in use.

  

![[Screenshot_2024-08-04_at_6.16.07_PM.png]]

  

## Conclusion

Is JS a good language?

Yes and no. It is beginner friendly, but has a lot of performance overhead. **Bun** is trying to solve for a lot of this, but there’s a long way to go before JS can compete with languages like C++/Rust