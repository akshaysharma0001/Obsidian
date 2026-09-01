```javascript
function setTimeoutP(ms){
    return new Promise((resolve=>{
        setTimeout(resolve,ms)
    }))
}
async function timer(){
    await setTimeoutP(5000)
    console.log("hi there")

    await setTimeoutP(3000)
    console.log("hello there")

    await setTimeoutP(1000)
    console.log("How are you")
}
timer()
```

