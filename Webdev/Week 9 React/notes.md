userState- return a statevariable and a function that modivies that variable 
mounting , re-rendering,unmounting
useState 

```javascript
import { useState, useEffect } from 'react'

import './App.css'

//Conditional rendering

function App() {

  let [counterVisible,setCounterVisible]=useState(true)
//mounting logic
  useEffect(()=>{
    setInterval(()=>{
      setCounterVisible(c=>!c)
    },5000)

    
  },[])


  return (  
  <div>
    hi
    {counterVisible && <Counter/>}
    hello
  </div>

  )
}

function Counter() {
  const [count, setCount] = useState(0)

  /*hooking into lifecycle of events of react

  mounting,rendering ,unmounting

  When we use useEffect the function inside useEffect only mount once
  If we remove it our clock will go crazy
  gaurds our setInterval from reRender
  */ 

  //mounting logic
  useEffect(() => {
    console.log("mounting")
    let clock = setInterval(()=>{
      console.log("from set interval")
      setCount(count=>count+1)
    }, 1000);

    //unmounting logic
    return ()=>{
      console.log("unmounting")
      clearInterval(clock)}
    
  }, [])//dependency array ,cleanup,fetch inside userEffect

  return (
    <div>
      <h1>{count}</h1>
    </div>
  )
}

export default App

```