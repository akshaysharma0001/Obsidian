### Creating a DOM element which has another DOM element inside

Lets write some code in which you have a button. When you click on a button, a slightly complex DOM element gets appended to the DOM.

```html
<div>
	<h1>hi there<h1>
</div>
```

### Approach #1

```html
<body>
	<button onclick="createComplexDomElement()">Add</button>
</body>
<script>
	function createComplexDomElement() {
		const div = document.createElement("div");
		div.innerHTML = "<h1> hi there </h1>";
		document.querySelector("body").appendChild(div);
	}
</script>
```