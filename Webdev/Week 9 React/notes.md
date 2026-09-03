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


```javascript
import { useState, useEffect } from 'react'

import './App.css'

//Conditional rendering

function App() {

  return(
    <div style={{display:"flex",justifyContent:'center'}}>
      <div style={{display:"flex",justifyContent:'center',flexDirection:'column'}}>
        <div style={{display:'flex'}}>
          <StoriesComponent user={"https://scontent-bom5-1.cdninstagram.com/v/t51.82787-19/791683616_18410814274153641_2908680310961580846_n.jpg?stp=dst-jpg_s150x150_tt6&_nc_cat=111&ccb=7-5&_nc_sid=f7ccc5&efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLnd3dy44ODcuQzMifQ%3D%3D&_nc_ohc=QtbTE2KNIJ4Q7kNvwG3o3Mc&_nc_oc=Adp37DdDQJ0NkyzCNPbJ4RQcuuElP6DMIphTFaxMyxgR0ptMAWdVlbBaqDOUD_vhbO9nM7z1uK2uCNteM9_BhJgm&_nc_zt=24&_nc_ht=scontent-bom5-1.cdninstagram.com&_nc_gid=RPBZ9gFQ7R9EWoswO13PJw&_nc_ss=796a8&oh=00_AQKqwLvGRPoWCOvQfuYeEVhVChqB1yCuNU4_mFvj26tFLQ&oe=6A9E0C10"}/>
          <StoriesComponent user={"https://scontent-bom5-1.cdninstagram.com/v/t51.82787-19/620947037_17939087889126346_477504865836222548_n.jpg?stp=dst-jpg_s206x206_tt6&_nc_cat=109&ccb=7-5&_nc_sid=bf7eb4&efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLnd3dy4xMDgwLkMzIn0%3D&_nc_ohc=f0WuVCKynBQQ7kNvwH_AmKo&_nc_oc=AdrNMHXWE9eGkKuSESScwaOnEw9gUVWFUAQ0JkKqW64aZwKOB8bsPO-L-9Z7v500AYPOuZJSvAzqibrvQvbUlDcP&_nc_zt=24&_nc_ht=scontent-bom5-1.cdninstagram.com&_nc_gid=Bxc_8xplYS3n9nJqs3a9hw&_nc_ss=7b6a8&oh=00_AQLzMzt9L2kbOdio43v6hzBOXCB-q9MyjarpcSjM-_XXYw&oe=6A9E2D87"}/>
          
        </div>
    
      <PostComponent logo={"https://scontent-bom2-3.cdninstagram.com/v/t51.2885-19/173266004_472065277374286_8628022641223545958_n.jpg?stp=dst-jpg_s206x206_tt6&_nc_cat=101&ccb=7-5&_nc_sid=bf7eb4&efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLnd3dy4xMDgwLkMzIn0%3D&_nc_ohc=ygIAw1nS_mEQ7kNvwEdz6uA&_nc_oc=Adr6gHjWDcbbNJIKGZghXzWsEHQUP9zj-dsYH-O95PH6WLH3ciKSRhNDhrQz8acVgjgLpPJyfNrn_zfdm6NE3x8M&_nc_zt=24&_nc_ht=scontent-bom2-3.cdninstagram.com&_nc_ss=7b6a8&oh=00_AQLQcIRs23j_65yKg2R71UNj7UQvGAxBeTMRz8CzhHNqtw&oe=6A9E25A6"}
      
      post={"https://scontent-bom2-4.cdninstagram.com/v/t51.82787-15/790475959_18479364874110775_2073283831845381871_n.heic?stp=dst-jpg_e35_tt6&_nc_cat=106&ig_cache_key=Mzk3NzE4MDI0ODA1MDM0NjQwMg%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkZFRUQueHBpZHMuMTQ0MC5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=Gdu5rbn37yAQ7kNvwEWv2qJ&_nc_oc=Adqh4GXJW-_rUzsnJoeXxejHpzd-udpjFuAdz6DaNA9PAXRYnyWc7hTdBQKIsxWgSxRCH3clnwviVwhMUqafkzsr&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-bom2-4.cdninstagram.com&_nc_gid=coTXa06OM6_tvKDi5jnaBw&_nc_ss=7a22e&oh=00_AQIsQPjk5VgM-jOOTF0hPeM4ShC5t6Ow5kwpHc_hs4_MeA&oe=6A9DFC35"}

      title={"GLAU"}
      />

      <PostComponent logo={"https://scontent-bom5-2.cdninstagram.com/v/t51.82787-19/691192315_18108312607729054_2969180855701412874_n.jpg?stp=dst-jpg_s150x150_tt6&_nc_cat=102&ccb=7-5&_nc_sid=f7ccc5&efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLnd3dy4xMDgwLkMzIn0%3D&_nc_ohc=e7lAJ_ce4usQ7kNvwHJhtS7&_nc_oc=AdoKUzk_NcUEsyXiD2ZHRFUXgrLKsEyn4xfhsTWADaiJ8tDeCnJwr8gUnNR1YdYYm46v3iCkyJbBAKgmlnjJgHwu&_nc_zt=24&_nc_ht=scontent-bom5-2.cdninstagram.com&_nc_gid=QFjnHKv5BEgBIlMhTVc3UQ&_nc_ss=7b6a8&oh=00_AQJlHmN6oxrtC9UQ3UqV0thDCSC1zJO7UgVg_SUZZK-Iog&oe=6A9E2271"
        
      }

      post={"https://scontent-bom5-2.cdninstagram.com/v/t51.82787-15/735446756_18135343090580939_8573313754301142170_n.jpg?stp=dst-jpg_e35_p750x750_sh2.08_tt6&_nc_cat=102&ig_cache_key=MzkzNTk5ODk3OTI0NDg4NzMyMg%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkNBUk9VU0VMX0lURU0ueHBpZHMuMzAyMy5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=J9y0aU7DEYwQ7kNvwHXVo7p&_nc_oc=AdoUIPAMsING9W8pyaTrgdArKY4B7DV2YGmfRGG2SBpbdoI8_B3KeTchTSq8U5j-zpXuroHD3PRFMompOtn2yv3y&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-bom5-2.cdninstagram.com&_nc_gid=pmRvtVwkaAzN-L5AgDOWeA&_nc_ss=7a22e&oh=00_AQLNHTzyBbNC6g4wOQ27oh4Xj16qrSjnkhztTxSEeBcwrQ&oe=6A9E0520"}

      title={"Apsani"}
      />

      <PostComponent logo={"https://scontent-bom5-2.cdninstagram.com/v/t51.82787-19/691192315_18108312607729054_2969180855701412874_n.jpg?stp=dst-jpg_s150x150_tt6&_nc_cat=102&ccb=7-5&_nc_sid=f7ccc5&efg=eyJ2ZW5jb2RlX3RhZyI6InByb2ZpbGVfcGljLnd3dy4xMDgwLkMzIn0%3D&_nc_ohc=e7lAJ_ce4usQ7kNvwHJhtS7&_nc_oc=AdoKUzk_NcUEsyXiD2ZHRFUXgrLKsEyn4xfhsTWADaiJ8tDeCnJwr8gUnNR1YdYYm46v3iCkyJbBAKgmlnjJgHwu&_nc_zt=24&_nc_ht=scontent-bom5-2.cdninstagram.com&_nc_gid=QFjnHKv5BEgBIlMhTVc3UQ&_nc_ss=7b6a8&oh=00_AQJlHmN6oxrtC9UQ3UqV0thDCSC1zJO7UgVg_SUZZK-Iog&oe=6A9E2271"}
      
      post={"https://scontent-bom5-1.cdninstagram.com/v/t51.82787-15/789033003_18125769769729054_7889884037059722259_n.heic?stp=dst-jpg_e35_tt6&_nc_cat=110&ig_cache_key=Mzk3NTE2NjI0NTk2Mzg3MTUxMQ%3D%3D.3-ccb7-5&ccb=7-5&_nc_sid=58cdad&efg=eyJ2ZW5jb2RlX3RhZyI6IkNBUk9VU0VMX0lURU0ueHBpZHMuMTQ0MC5zZHIucmVndWxhcl9waG90by5DMyJ9&_nc_ohc=HpIx7UPsU5kQ7kNvwHh623D&_nc_oc=AdqKWmdUp_SMBLl9O8GoAdl2AQk3K6MIY9XcUyvcKDLerDxZM7fsnN4J1SW2A-UxNJ2G94Y6cQd4WAxUUZVN_LqY&_nc_ad=z-m&_nc_cid=0&_nc_zt=23&_nc_ht=scontent-bom5-1.cdninstagram.com&_nc_gid=Fh4YH4Ze5pqW0_gtRpi7Cw&_nc_ss=7a22e&oh=00_AQLF4-6pOY9BeriwiztQiiar7jFIkT_SKJFkYTh_nLRXgQ&oe=6A9E17BB"}

      title={"Avantika"}
      
      />
    </div>
    </div>
  )
}

 function PostComponent({logo,post,title}){
  return(
    <div style={{display:"flex",justifyContent:'center',flexDirection:"column",width:'fitContent',backgroundColor:'grey',padding:"20px",borderRadius:'30px',margin:'10px'}}>
      <div>
        <img src={logo} style={{width:"30px",height:'30px',borderRadius:'30px'}} />
      <b>{title}</b> 7h
      </div>
      
      <div>
        <img src={post} style={{width:"430px",height:'600px',borderRadius:'30px'}}/>
      </div>
    </div>
  )
 }

 function StoriesComponent({user}){
  return(

      <div >
        <img src={user} style={{width:"80px",height:'80px',borderRadius:'80px',border:'solid #fc03f8 5px',margin:'5px'}}  />
      </div>

  )
 }

export default App

```


```javascript
import { useState, useEffect } from 'react'

import './App.css'

//Conditional rendering

function App() {

  //Conditional rendering
  return (
    <div>
      <br />
    <ToggleMessage/>

    <ToggleMessage/>

    <ToggleMessage/>

    </div>
  )
  
}

const ToggleMessage=()=>{

  const[isVisible,setVisible]=useState(true)

  function toggle(){
    setVisible(!isVisible)
  }

  return(
    <div>
      <button onClick={toggle}>Toggle message</button>
      <h1>{isVisible?"This message is condidionally rendered":null}</h1>
    

    </div>

  )
}



export default App

```