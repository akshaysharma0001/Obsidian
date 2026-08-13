- removeChild - Removes a specific `node` of a `parent`
- onclick - function that triggers whenever you `click` on a button

### Assignment - Add a `delete` button right next to the `todo` that deletes that todo

```jsx
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>replit</title>
  <link href="style.css" rel="stylesheet" type="text/css" />
</head>

<body>
  <h1>Todo list</h1>
  <div>
    <div id="todo-1">
      <h4>1. Take class</h4>
      <button onclick="deleteTodo(1)">delete</button>
    </div>
    <div id="todo-2">
      <h4>2. Go out to eat</h4>
      <button onclick="deleteTodo(2)">delete</button>
    </div>
  </div>
  <div>
    <input type="text"></input>
    <button>Add Todo</button>
  </div>
</body>

<script>
  function deleteTodo(index) {
    const element = document.getElementById("todo-" + index);
    element.parentNode.removeChild(element);
  }
</script>

</html>
```

  

  

Another experiment we did in class -

```jsx
<html>
    <body id="body">
        <h2>Todo 1</h2>
        <h2>Todo 2</h2>
        <h2>Todo 3</h2>
        <button onclick="deleteRandomTodo()">Delete todo!</button>
    </body>
    <script>
        function deleteRandomTodo() {
            const element = document.querySelector("h2");
            const parentElement = element.parentNode;
            parentElement.removeChild(element);
        }

    </script>
</html>
```