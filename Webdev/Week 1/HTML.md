Elements /tags
- Every tag can have attribute
- `<body style="background-color:blue;">`
- ==Important attributes==- font-size, background-color, color ,text align 
- Important HTML tags - 
	-  `<b>` - Bold text
	- `<strong>` - Important text- it appears bold
	- `<i>` - Italic text
	- `<em>` - Emphasized text -its like italics
	- `<mark>` - Marked text -highlighted
	- `<small>` - Smaller text
	- `<del>` - Deleted text -strike through
	- `<ins>` - Inserted text -underlined
	- `<sub>` - Subscript text
	- `<sup>` - Superscript text
	- `<a>`- > `<a href="_url_">_link text_</a>`
		- The `target` attribute can have one of the following values:
		- `_self` - Default. Opens the document in the same window/tab as it was clicked
		- `_blank` - Opens the document in a new window or tab
		- `_parent` - Opens the document in the parent frame
		- `_top` - Opens the document in the full body of the window
	- `<img>`-`<img src="img_girl.jpg" alt="Girl in a jacket">`

### Table
- th- table heading
- td - table data
- tr -table row
- add border - `table,th,td { border:1px solid black; }`
- adjust column width -`<th style="width: 20%;">Name</th>`


![[Table example.png]]

- Code ->
		`<table>
        <tr>
            <th rowspan="2" colspan="2" style="width: 50%;">Name</th>
            <th colspan="2">Marks</th>
            
        </tr>
        <tr>
            
            <td>java</td>
            <td>python</td>
            
        </tr>
        <tr>
            <td>Akshay</td>
            <td>sharma</td>
            <td>85</td>
            <td>95</td>
        </tr>
        <tr>
            <td>Ayush</td>
            <td>pathak</td>
            <td>82</td>
            <td>89</td>
        </tr>
        <tr>
            <td>Ansh</td>
            <td>pal singh</td>
            <td>80</td>
            <td>84</td>
        </tr>
    </table>`