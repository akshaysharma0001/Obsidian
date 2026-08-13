## Objects

An object in JavaScript is a collection of `key-value pairs`, where each `key` is a string and each `value` can be any valid JavaScript data type, including another object.

  

![[Screenshot_2024-08-04_at_6.43.02_PM.png]]

  

```jsx
let user = {
	name: "Harkirat",
	age: 19
}

console.log("Harkirats age is " + user.age);
```

Assignment #1

Write a function that takes a `user` as an input and greets them with their name and age

Assignment #2

Write a function that takes a new object as input which has `name` , `age` and `gender` and greets the user with their gender (Hi `Mr/Mrs/Others` harkirat, your age is 21)

Assignment #3

Also tell the user if they are legal to vote or not

## Arrays

Arrays let you group data together

```jsx
const users = ["harkirat", "raman", "diljeet"];
const tatalUsers = users.length;
const firstUser = users[0];
```

Assignment

Write a function that takes an array of numbers as input, and returns a new array with only even values. Read about `filter` in JS

## Array of Objects

We can have more complex objects, for example an array of objects

```jsx
const users = [{
		name: "Harkirat",
		age: 21
	}, {
		name: "raman",
		age: 22
	}
}

const user1 = users[0] 
const user1Age = users[0].age
```

Assignment

Write a function that takes an array of users as inputs and returns only the users who are more than 18 years old

## Object of Objects

We can have an even more complex object (object of objects)

```jsx
const user1 = {
	name: "harkirat",
	age: 19,
	address: {
		city: "Delhi",
		country: "India",
		address: "1122 DLF"
	}
}

const city = user1.address.city;
```

Assignment

Create a function that takes an array of objects as input,  
and returns the users whose age > 18 and are male