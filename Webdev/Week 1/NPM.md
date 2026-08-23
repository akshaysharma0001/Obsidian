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