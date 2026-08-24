```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>to do list</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <center>    
        <h1>Todo list</h1>
        <div class="parent">
        </div>
        <input type="text" id="task" ><button onclick="addTodo()">Add item</button>
    </center>
<script src="script.js"></script>
</body>
</html>
```

```javascript
let todo=[]

render()

  

function addTodo(){

let inp=document.getElementById("task")

todo.push({"id":todo.length,

    "task":inp.value.trim(),

    "done":false

})

inp.value=""

render()

}

  

function deleteTodo(id){

    todo.pop(id)

    render()  

}

  

function moveUp(id){

    if(id==0){

        alert("Not possible")

    }

    else{

        let t1=todo[id]

        t1.id=id-1

        let t2=todo[id-1]

        t2.id=id

        todo[id]=t2

        todo[id-1]=t1

    }

    render()

  

}

  

function moveDown(id){

    if(id==todo.length-1){

        alert("Not possible")

    }

    else{

        let t1=todo[id]

        let t2=todo[id+1]

        t1.id=id+1

        t2.id=id

        todo[id+1]=t1

        todo[id]=t2

    }

    render()

  

}

function component(item,i){

    let parent=document.querySelector(".parent")

  

    let div=document.createElement("div")

    div.className="child"

    div.setAttribute("id",i)

    let chkbx=document.createElement("input")

    chkbx.type="checkbox"

  

    let task=document.createElement("h3")

    task.innerText=item.task

    chkbx.addEventListener('change',()=>{

        if(chkbx.checked){

            task.style.textDecoration="line-through"

        }

        else{

            task.style.textDecoration="none"

        }

    })

  

    let btn1=document.createElement("button")

    btn1.innerText="Delete"

    btn1.onclick=()=>{deleteTodo(i)}

  

    let btn2=document.createElement("button")

    btn2.innerText="Move up"

    btn2.onclick=()=>{moveUp(i)}

  

    let btn3=document.createElement("button")

    btn3.innerText="Move down"

    btn3.onclick=()=>{moveDown(i)}

  

    div.appendChild(chkbx)

    div.appendChild(task)

    div.appendChild(btn1)

    div.appendChild(btn2)

    div.appendChild(btn3)

    parent.appendChild(div)

}

function render(){

    let parent=document.querySelector(".parent")

    parent.innerHTML=""

    for(let i=0;i<todo.length;i++){

        component(todo[i],i)

    }

  

}
```