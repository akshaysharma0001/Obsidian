There are 5 popular methods available for fetching DOM elements -

- querySelector
- querySelectorAll
- getElementById
- getElementByClassName
- getElementsByClassName

### 1. Fetching the title

![[Screenshot_2024-08-17_at_5.39.48_PM.png]]

```jsx
const title = document.querySelector('h1');
console.log(title.innerHTML)
```

  

### 2. Fetching the first TODO (Assignment)

![[Screenshot_2024-08-17_at_5.42.34_PM.png]]

```jsx
const firstTodo = document.querySelector('h4');
console.log(firstTodo.innerHTML)
```

  

### 3. Fetching the `second` TODO (Assignment)

![[Screenshot_2024-08-17_at_5.44.15_PM.png]]

  

```jsx
const secondTodo = document.querySelectorAll('h4')[1];
console.log(secondTodo.innerHTML)
```