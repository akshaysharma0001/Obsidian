### Java
**1. What is the difference between JDK, JRE, and JVM?**

**A:** JDK is the full developer kit (contains compiler). JRE is the environment to run the code (contains libraries). JVM is the virtual machine that actually executes the bytecode, making Java platform-independent.

  

**2. How does Java manage memory differently than C++?**

**A:** Java does not use manual pointers for memory management. It uses an automatic Garbage Collector to destroy unused objects in the Heap, preventing memory leaks.

  

**3. What is the difference between Stack and Heap memory?**

**A:** The Stack stores local variables and method calls (fast, temporary). The Heap stores all objects created with the `new` keyword (large, dynamic).

  

**4. Can you force Garbage Collection in Java?**

**A:** No. You can request it using `System.gc()`, but the JVM ultimately decides when to actually run the Garbage Collector.

  

### Object-Oriented Programming (OOP)

**5. What are the four pillars of OOP?**

**A:** Encapsulation (hiding data), Abstraction (hiding complexity), Inheritance (reusing code), and Polymorphism (one method, many forms).

  

**6. What is the difference between Method Overloading and Method Overriding?**

**A:** Overloading (Compile-time): Same method name, different parameters in the same class. Overriding (Run-time): A child class redefines a method inherited from its parent class.

  

**7. Abstract Class vs. Interface?**

**A:** An Abstract Class can have both regular methods and abstract (empty) methods. An Interface (pre-Java 8) only has abstract methods. A class can implement multiple interfaces but can only extend one abstract class.

  

**8. How do you achieve Encapsulation?**

**A:** By declaring class variables as `private` and providing `public` getter and setter methods to access them safely.

  

### Keywords

**9. What does the `static` keyword do?**

**A:** It attaches a variable or method to the Class itself, rather than to an individual object. It is shared across all instances of that class.

  

**10. What does the `final` keyword do?**

**A:** On a variable, it makes it a constant. On a method, it prevents overriding. On a class, it prevents inheritance.

  

**11. What is the difference between `this` and `super`?**

**A:** `this` refers to the current object instance. `super` refers to the immediate parent class object (used to call parent constructors or methods).

  

### Strings

**12. Why are Strings immutable in Java?**

**A:** For security and memory efficiency. Java stores them in a "String Pool." If you change a String, it doesn't edit the original; it creates a brand-new object in memory.

  

**13. What is the difference between `String` and `StringBuilder`?**

**A:** `String` is immutable (creates new objects on modification). `StringBuilder` is mutable (modifies the same object), making it much faster for string manipulation loops.

  

**14. What is the difference between `==` and `.equals()`?**

**A:** `==` checks if two references point to the exact same memory address. `.equals()` checks if the actual text/content inside the objects is identical.

  

### Exception Handling

**15. What is the difference between Checked and Unchecked Exceptions?**

**A:** Checked exceptions (like `IOException`) are checked by the compiler and must be handled with a `try-catch` block before the code runs. Unchecked exceptions (like `NullPointerException`) happen at runtime due to bad logic.

  

**16. What is the purpose of the `finally` block?**

**A:** It is a block of code placed after `try-catch` that _always_ executes, whether an exception occurred or not. It is typically used to close database connections or files.

  

**17. What is a `NullPointerException`?**

**A:** A runtime error that occurs when you try to call a method or access a variable on an object reference that currently points to `null` (nothing).

  

### Collections (Data Structures)

**18. What is the Java Collections Framework?**

**A:** A set of classes and interfaces (like List, Set, Map) that provide standardized ways to store and manipulate groups of objects.

  

**19. What is the difference between `ArrayList` and `LinkedList`?**

**A:** `ArrayList` uses a dynamic array (fast for searching, slow for inserting in the middle). `LinkedList` uses nodes with pointers (fast for inserting, slow for searching).

  

**20. What is the difference between a `HashSet` and a `TreeSet`?**

**A:** `HashSet` stores unique items in random order (very fast). `TreeSet` stores unique items sorted in ascending order (slightly slower).


## Mern

### MongoDB (Database)

**1. What is the difference between SQL and NoSQL?**

**A:** SQL is table-based with rigid schemas (relational). NoSQL (like MongoDB) is document-based, storing data in flexible, JSON-like structures (BSON), allowing for rapid iteration.

  

**2. What are Collections and Documents in MongoDB?**

**A:** A Collection is the equivalent of a SQL Table. A Document is the equivalent of a SQL Row, but it is stored as a JSON object (key-value pairs).

  

**3. Why use Zod with MongoDB?**

**A:** MongoDB is schema-less by default. Zod acts as a runtime validation boundary on your Express backend, ensuring malformed or malicious data (like a string instead of a number) never reaches the database.

  

**4. What is an Aggregation Pipeline?**

**A:** A framework for data processing in MongoDB. Documents enter a multi-stage pipeline that transforms them into aggregated results (e.g., filtering out users, then grouping them by city, then calculating an average score).

  

### Node.js & Express (Backend)

**5. How does Node.js handle concurrency if it is single-threaded?**

**A:** It uses an event-driven, non-blocking I/O model. Intensive tasks (like reading files or database queries) are offloaded to the operating system or a worker pool, while the main Event Loop keeps accepting new requests.

  

**6. What is Express Middleware?**

**A:** Functions that run between receiving a request and sending a response. They are used for tasks like logging, parsing JSON bodies, or checking authentication before hitting the final route.

  

**7. How does JWT (JSON Web Token) authentication work?**

**A:** The server verifies login credentials and issues a signed token (JWT). The client sends this token in the header of future requests. The server mathematically verifies the signature to authenticate the user without maintaining a session state.

  

**8. What is CORS?**

**A:** Cross-Origin Resource Sharing. A browser security feature that prevents a malicious website from making API requests to a different domain. You must explicitly configure your Express backend to allow requests from your React frontend URL.

  

**9. What is the difference between `PUT` and `PATCH`?**

**A:** `PUT` replaces the entire existing resource. `PATCH` applies partial modifications to a resource (e.g., updating just the password).

  

**10. How do you securely store passwords?**

**A:** Never store them in plain text. Use a hashing algorithm like `bcrypt` to convert the password into a one-way, irreversible hash before saving it to the database.

  

### React (Frontend)

**11. What is the Virtual DOM?**

**A:** A lightweight, in-memory representation of the actual browser DOM. React compares the new Virtual DOM with the old one (Diffing) and updates only the specific elements that changed on the real screen, making it extremely fast.

  

**12. What is Prop Drilling and how do you avoid it?**

**A:** Prop drilling is passing data through multiple layers of components that don't need the data themselves just to get it to a deeply nested child. You avoid it using state management libraries like Jotai, Redux, or the React Context API.

  

**13. Why use Jotai over Context API?**

**A:** Jotai uses atomic state (independent bubbles of state). This prevents unnecessary re-renders of the entire component tree, which is a common performance issue when using standard React Context for complex data.

  

**14. What is the difference between `useState` and `useRef`?**

**A:** `useState` stores a value and triggers a component re-render when it changes. `useRef` stores a mutable value that persists across renders but does _not_ trigger a re-render when changed.

  

**15. What is the `useEffect` dependency array?**

**A:** An array `[]` passed as the second argument to `useEffect`. It tells React when to run the effect. If empty, it runs once on mount. If it contains a variable `[count]`, it runs every time `count` changes.

  

**16. What are Controlled vs. Uncontrolled Components?**

**A:** In a controlled component, form data (like an input field) is handled by React state (`useState`). In an uncontrolled component, form data is handled by the DOM itself (accessed via `useRef`).

  

### JavaScript Core

**17. What is the Event Loop?**

**A:** The mechanism that allows JavaScript to perform non-blocking operations. It pushes asynchronous callbacks (like `setTimeout` or API calls) to a queue and executes them only when the main call stack is empty.

  

**18. What is the difference between `==` and `===`?**

**A:** `==` checks for value equality but performs type coercion (converts types to match, so `"5" == 5` is true). `===` checks for both value and strict type equality (`"5" === 5` is false).

  

**19. What is a Closure?**

**A:** A function that remembers the variables from its outer (lexical) scope even after the outer function has finished executing.

  

**20. What is Hoisting?**

**A:** JavaScript's default behavior of moving variable (`var`) and function declarations to the top of their scope before code execution. Variables declared with `let` and `const` are hoisted but remain in the "Temporal Dead Zone" and cannot be accessed before initialization.

  

You have the knowledge, the projects, and the preparation. Go crush this interview! Do you need a quick rundown on your "Tell me about yourself" pitch before you walk in?