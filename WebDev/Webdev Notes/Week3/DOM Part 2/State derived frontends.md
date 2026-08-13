To make frontends easier to code, the concept of `state` came into the picture. You will see this more when we reach react.

There are three `jargon` we need to understand

1. State - The `variable` parts of an app.
2. Components - How to `render` `state` on screen.
3. Rendering - Taking the `state` and rendering it on the `DOM` based on the `components`

![[Screenshot_2024-08-18_at_4.35.34_PM.png]]

  

## TODO App

### State

```jsx
const todos = [{
   id: 1,
   description: "Go to gym"
}, {
   id: 2,
   description: "Eat food"
}];
```

### Component

```jsx
function todoComponent(todo) {
	const div = document.createElement("div");
	const h1 = document.createElement("h1");
	const button = document.createElement("button");
	button.innerHTML = "Delete";
	h1.innerHTML = todo.title;
	div.appendChild(h1);
	div.appendChild(button);
}
```

![[Screenshot_2024-08-18_at_4.40.01_PM.png]]

  

## Linkedin Topbar

![[Screenshot_2024-08-18_at_6.12.31_PM.png]]

### State

```jsx
const state = {
    notifications: {
	    home: 0,
	    myNetwork: "99+",
	    jobs: 0,
	    messaging: 0,
	    notifications: 25
    },
    profilePicture: "https://media.licdn.com/dms/image/v2/C5603AQFbOqG9og1S5g/profile-displayphoto-shrink_100_100/profile-displayphoto-shrink_100_100/0/1517251238138?e=1729728000&v=beta&t=xHUuE_3gkUPXYajsv8fk_kv37oB49Mqbi20IVAjn_rw"
}
```

  

### Components

![[Screenshot_2024-08-18_at_6.15.11_PM.png]]

  

  

### Started code

```jsx
<body>
  <input type="text"></input>
  <button onclick="addTodo()">Add todo!</button>
  <script>
    let todos = [];
    function addTodo() {
      todos.push({
        title: document.querySelector("input").value
      })
      render();
    }

    function deleteTodo() {
      
      render();
    }
 
    function render() {
      
    }
  </script>
</body>
```