> A **Promise** in JavaScript is an object that represents the `eventual completion` (or failure) of an asynchronous operation and its resulting value. Promises are used to handle asynchronous operations more effectively than traditional callback functions, providing a cleaner and more manageable way to deal with code that executes asynchronously, such as API calls, file I/O, or timers

- In promise class we have to pass a function which have functions as a parameter reject,resolve.
- When the Async operation is completed then it calls the resolve function 
- Resolve function points to the callback function which is user defined

```javascript
function setTimeoutPromisified(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function callback() {
	console.log("3 seconds have passed");
}

setTimeoutPromisified(3000).then(callback)

```

```javascript
const fs=require('fs')
function cleanFile(filename){
    return new Promise((reject,resolve)=>{
        let data=fs.readFile(filename,"utf-8",(err,data)=>{
            if(err){
                reject(err)
            }
            else{
                data=data.trim()
                resolve(data)
            }
        })
    })
}

  

cleanFile("x.txt").then((data)=>{console.log(data)}).catch((err)=>{console.log(err)})
```


![[Promise calling.png]]

# Promise States

A Promise always exists in one of **three mutually exclusive states**: 
- ⏳ **Pending**: The initial state. The asynchronous operation is still running, and the promise is neither fulfilled nor rejected.

- ✅ **Fulfilled**: The operation completed successfully, and the promise now holds a resulting value. 

- ❌ **Rejected**: The operation failed, and the promise holds a reason or error for the failure. [[1]

Once a promise is either fulfilled or rejected, it is considered **settled**. A settled promise's state is terminal and cannot change again.