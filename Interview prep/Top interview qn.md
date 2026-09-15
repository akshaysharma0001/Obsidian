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

  

Take a deep breath, review these quickly, and walk in with confidence. You are going to do great today! Let me know if you need to clarify one of these before you walk into the room.