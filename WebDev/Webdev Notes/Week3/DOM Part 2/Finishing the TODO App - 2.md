Let’s look at a slightly better approach of doing the same thing.

### Creating a DOM element which has another DOM element inside

  

```html
<body>
	<button onclick="createComplexDomElement()">Add</button>
</body>
<script>
	function createComplexDomElement() {
		const div = document.createElement("div");
		const h1 = document.createElement("h1");
		h1.innerHTML = "random text";
		div.appendChild(h1);
		document.querySelector("body").appendChild(div);
	}
</script>
```

  

![[Screenshot_2024-08-18_at_3.37.33_PM.png]]

  

![[Screenshot_2024-08-18_at_3.38.05_PM.png]]