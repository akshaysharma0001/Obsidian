### Properties of JS
1. ==Interpreted== -JavaScript is an interpreted language, meaning it's executed line-by-line at runtime by the JavaScript engine
2. ==Dynamically Typed==- Variables in JavaScript are not bound to a specific data type. Types are determined at runtime and can change as the program executes
3. ==Single threaded==
4. ==Garbage collected== -JavaScript automatically manages memory allocation and deallocation through garbage collection

- Node.js is an open-source `JS runtime` that allows you to execute JavaScript code on the server side. It’s built on Chrome's V8 JavaScript engine.

- The ==V8 engine== is an open-source JavaScript engine developed by Google. It is used to execute JavaScript code in various environments, most notably in the Google Chrome web browser

### Objects
- An object in JavaScript is a collection of `key-value pairs`, where each `key` is a string and each `value` can be any valid JavaScript data type, including another object.
- `let user = {`
			`name: "Akshay",`
			`age: 19`
			`}`
`console.log("Harkirats age is " + user.age);`



![[package json file.png]]

### npm
- The full form of **NPM** is **Node Package Manager**
- It is a package manager for JavaScript, primarily used for managing libraries and dependencies in Node.js projects. NPM allows developers to easily install, update, and manage packages of reusable code

- initializing a project `npm init -y`
- running scripts `npm run test`
- installing external dependencies `npm install chalk`

- Node.js provides you some `packages` out of the box. Some common ones include
	1. fs - Filesystem
	2. path - Path related functions
	3. http - Create HTTP Servers

### Semantic Versioning Format
The format is as follows - `MAJOR.MINOR.PATCH`
- ==MAJOR== - Major version changes indicate significant updates or breaking changes.

- ==MINOR== - Minor version changes signify the addition of new features or improvements in a backward-compatible manner.

- ==PATCH== - Patch version changes include backward-compatible bug fixes or minor improvements that address issues without adding new features or causing breaking changes.

#### Usage in `package.json`
- `“chalk”: “^5.3.0”` - npm will install any version that is compatible with `5.3.0` but less than `6.0.0`. This includes versions like `5.3.1`, `5.4.0`, `5.5.0`, etc.

- `“chalk”: “5.3.0”` - Will install the exact version

- `"chalk": "latest"` - Will install the latest version

- The `package-lock.json` records the exact versions of all dependencies and their dependencies (sub-dependencies) that are installed at the time when `npm install` was run.


### Classes














