#### Primitive types
1. number
2. string
3. boolean

#### Complex types
1. Objects
2. Arrays

#### Classes
- In JavaScript, classes are a way to define blueprints for creating objects (these objects are different from the objects defined in the last section).

For example

```javascript
class Rectangle {
   constructor(width, height, color) {
	    this.width = width;
	    this.height = height;
	    this.color = color; 
   }
   
   area() {
	   const area = this.width * this.height;
		 return area;
   }
   
   paint() {
			console.log(`Painting with color ${this.color}`);
   }
   
}

const rect = new Rectangle(2, 4)
const area = rect.area();
console.log(area)
```

#### Key Concepts
1. **Class Declaration**:
- You declare a class using the `class` keyword.
- Inside a class, you define properties (variables) and methods (functions) that will belong to the objects created from this class.

2. **Constructor**:
- A special method inside the class that is called when you create an instance (an object) of the class.
- It’s used to initialize the properties of the object.

3. **Methods**:
- Functions that are defined inside the class and can be used by all instances of the class.

1. **[[Inheritance]]**:
- Classes can inherit properties and methods from other classes, allowing you to create a new class based on an existing one.

5. **Static Methods**:
- Methods that belong to the class itself, not to instances of the class. You call them directly on the class.

6. **Getters and Setters**:
- Special methods that allow you to define how properties are accessed and modified.

#### Maps class
```javascript
const map = new Map();
map.set('name', 'Alice');
map.set('age', 30);
console.log(map.get('name'));
```
