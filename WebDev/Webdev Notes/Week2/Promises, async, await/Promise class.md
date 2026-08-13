Calling a promise is easy, defining your own promise is where things get hard

  

A **Promise** in JavaScript is an object that represents the `eventual completion` (or failure) of an asynchronous operation and its resulting value. Promises are used to handle asynchronous operations more effectively than traditional callback functions, providing a cleaner and more manageable way to deal with code that executes asynchronously, such as API calls, file I/O, or timers.

  

### Using a function that returns a promise

Ignore the function definition of `setTimeoutPromisifed` for now

```jsx
function setTimeoutPromisified(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function callback() {
	console.log("3 seconds have passed");
}

setTimeoutPromisified(3000).then(callback)
```

![[Screenshot_2024-08-11_at_6.37.35_PM.png]]