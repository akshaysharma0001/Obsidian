### What is CSS?
- Cascading Style Sheets (CSS) is used to format the layout of a webpage
- ==Inline CSS==-> `<h1 style="color:blue;">A Blue Heading</h1>`
- ==Internal CSS== ->
	- <style>  
			body {background-color: powderblue;}  
			h1   {color: blue;}  
			p    {color: red;}  
		</style>
- ==External CSS== -> `<link rel="stylesheet" href="styles.css">`
- `margin`: Sets the outer space around an element.
- `padding`: Sets the inner space within an element.
- `border`: Sets the border around an element.
- ==Flexbox== `display:flex `
- ![[flexbox.png]]

### Grid
`display:grid`
- The `grid-template-rows` property specifies the number (and the heights) of the rows in a grid layout
- `div style="display: grid;grid-template-columns: auto auto auto;grid-template-rows: auto auto auto;"`  OR
- `style="display:grid;grid-template-columns:repeat(3,1fr);grid-template-rows:repeat(3,1fr)"`
- ==fr== -> fraction of space

![[grid.png]]
