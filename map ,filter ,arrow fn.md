
### Arrow function 
```javascript
const sum=(a,b)=>{
    return a+b
}
console.log(sum(10,20))
```

### map function
>Given an array return new array in which every value is multiplied by 2
[1,2,3,4,5] -> [1,4,9,16,25]

```javascript
function arrsq(val){
    return val*val
}

let arr=[1,2,3,4,5]
let newarr=arr.map(arrsq)
console.log(newarr)
//Output : [ 1, 4, 9, 16, 25 ]
```