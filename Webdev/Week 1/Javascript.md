### Properties of JS
1. ==Interpreted== -JavaScript is an interpreted language, meaning it's executed line-by-line at runtime by the JavaScript engine
2. ==Dynamically Typed==- Variables in JavaScript are not bound to a specific data type. Types are determined at runtime and can change as the program executes
3. ==Single threaded==
4. ==Garbage collected== -JavaScript automatically manages memory allocation and deallocation through garbage collection

- Node.js is an open-source `JS runtime` that allows you to execute JavaScript code on the server side. It’s built on Chrome's V8 JavaScript engine.

- The ==V8 engine== is an open-source JavaScript engine developed by Google. It is used to execute JavaScript code in various environments, most notably in the Google Chrome web browser

#### If else
```javascript
function canVote(age)
{
    if(age<18)
    {
        console.log("You cant vote")
    }
    else{
        console.log("Eligible to vote")
    }
}
```
### Classes in JS
#### Primitive types
1. number
2. string
3. boolean

#### Complex types
1. Objects
2. Arrays
### Objects
- An object in JavaScript is a collection of `key-value pairs`, where each `key` is a string and each `value` can be any valid JavaScript data type, including another object.
- ```javascript
  let user = {
			name: "Akshay",
			age: 19
			}
console.log(user.name + " age is " + user.age);
  ```

```javascript
function greet(user)=
{
    if(user.gender=="male"){
        console.log("Hi Mr "+user.name+" your age is "+user.age)
    }
    else if(user.gender=="female"){
    console.log("Hi Mrs "+user.name+" your age is "+user.age)
    }
    else{
        console.log("Hi LGBTQ+ "+user.name+" your age is "+user.age)
    }
    if(user.age>18)
     {
         console.log("You cant vote")
     }
     else
     {
	    console.log("Eligible to vote")
     }
}
```

#### Filter
```javascript
const vote=[
    {name:"Akshay",
        age:25
    },
    {name:"Rohan",
        age:45
    },
    {name:"Piyush",
        age:15
    },
    {name:"Abhay",
        age:66
    }
function filvote(user)
{
   return user.age>=18
}
let filtervoters=vote.filter(filvote)
for(let i=0;i<filtervoters.length;i++)
{
    console.log(elivote[i].name)
}
```
















