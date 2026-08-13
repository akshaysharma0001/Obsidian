Given a state variable called `todos`, can you write a function called `render` that takes this as an input and `renders` the current list of todos

Todos look something like this -

```jsx
const todos = [{
	id: 1,
	title: "Go to gym"
}, {
	id: 2,
	title: "Clean the car"
}]
```

### Boilerplate code

```jsx
<body>
  <div id="root"></div>
  <script>
    function render(todos) {
      // your code here
    }
  </script>
</body>
```

  

Approach #1 - Clean the screen everytime we re-render

```jsx
<body>
  <div id="root"></div>
  <script>
    function render(todos) {
      const todoList = document.getElementById('root');
      todoList.innerHTML = ''; // Clear the list

      todos.forEach(todo => {
        const div = document.createElement('div');
        const h1 = document.createElement('h4');
        h1.textContent = todo.title;
        div.appendChild(h1);
        div.setAttribute('data-id', todo.id);
        todoList.appendChild(div);
      });
    }
    render([{
      id: 1,
      title: "Go to gym"
    }, {
      id: 2,
      title: "Clean the car"
    }])
  </script>
</body>
```

There is a better approach —- You find the diff and only do `deletes` / `updates` / `additions` that are necessary. But that’ll boggle most folks heads so we’re not going there. The general goal should be to minimize the number of interactions in the DOM.  
React does this by using something called the virtual DOM.