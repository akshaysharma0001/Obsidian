CSS stands for Cascading Style Sheets. It is used to style our applications

You can add CSS to your HTML app by using -

1. The `style` attribute (inline styles)
2. In an external css file

  

### Approach #1 - Inline styles

Try updating the `body` tag in the last style as follows -

```rust
<body style="background-color: black;">
... rest of the code
</body>
```

### Approach #2 - External styles

1. Add a new file called index.css
2. Add the following code in it

```rust
body {
    background-color: black;
}
```

- Update index.html

```rust
<html>
	<title>
		Visual Studio Code - Code Editor
	</title>
	<link rel="stylesheet" href="index.css">
</html>
<body>
... rest of the code
</body>
```

![[Screenshot_2024-08-03_at_7.05.50_PM.png]]

  

### Common style attributes

- `color`: Sets the text color.
- `background-color`: Sets the background color.
- `font-size`: Sets the size of the text.
- `margin`: Sets the outer space around an element.
- `padding`: Sets the inner space within an element.
- `border`: Sets the border around an element.

### Flexbox

Flexbox is a CSS layout model designed to help with the arrangement of items within a container.

Update the website to the following -

```rust
<html>
	<title>
		Visual Studio Code - Code Editor
	</title>
</html>
<body>
	<div style="display: flex;">
		<div>Visual Studio Code</div>
		<a href="/">Docs</span> 
		<a href="/">Updates</span> 
		<a href="/">Blog</span> 
		<a href="/">API</span> 
		<a href="/">Extensions</span> 
		<a href="/">FAQs</span>
		<a href="/">Learn</span>
	</div>
	<div>
		<input type="text" placeholder="Search Docs">
		<button>Download</button>
	</div>
	<br/>

	<div>
		<a href="/">Version 1.82</a> is now available! Read about the new features and fixes from July.
	</div>

	<br/>
</body>
```

Notice that the elements are positioned right next to each other even though `Visual Studio code` is inside a `div`

### Justify content

Try experimenting with the `justify-centent` property

![[Screenshot_2024-08-03_at_7.20.00_PM.png]]

```rust
<html>
	<title>
		Visual Studio Code - Code Editor
	</title>
</html>
<body>
	<div style="display: flex; justify-content: space-between;">
		<div>Visual Studio Code</div>
		<a href="/">Docs</span> 
		<a href="/">Updates</span> 
		<a href="/">Blog</span> 
		<a href="/">API</span> 
		<a href="/">Extensions</span> 
		<a href="/">FAQs</span>
		<a href="/">Learn</span>
	</div>
	<div>
		<input type="text" placeholder="Search Docs">
		<button>Download</button>
	</div>
	<br/>

	<div>
		<a href="/">Version 1.82</a> is now available! Read about the new features and fixes from July.
	</div>

	<br/>
</body>
```

  

## Another example

```rust
<html>

</html>
<body>
    <header>

    </header>
    <section>
    <div style="border-width: thick; border-style: solid; display: flex; justify-content: space-between; margin-left: 200px; margin-right: 200px;">
        <div style="background: red; "> 
            <h1>
                Code with GitHub Copilot
            </h1>
            <h6>
                Write code faster and smarter with GitHub Copilot, your AI pair programmer.
            </h6>            
            Try GitHub Copilot free for 30 days
            Completions present suggestions automatically to help you code more efficiently.
            
            Copilot Chat understands the context of your code, workspace, extensions, settings, and more.
            
            Inline Chat enables you to iteratively generate edits and get answers to quick questions, directly on your code.
        </div>
        <div style="background: green;">
        <img src="https://code.visualstudio.com/assets/home/swimlane-copilot.png" width="800px" /></div>
    </div>
</section>
    <footer>

    </footer>
</body>
```

### Classes and ids

In CSS, classes and IDs are used as selectors to apply styles to HTML elements. They help in targeting specific elements for styling and can be used to enhance the modularity and reusability of CSS code.