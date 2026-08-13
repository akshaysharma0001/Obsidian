Create a `command line interface` that lets the user specify a file path and the nodejs process counts the number of words inside it.

```jsx
Input - node index.js /Users/kirat/file.txt
Output - You have 10 words in this file
```

  

Library to use - https://www.npmjs.com/package/commander

  

What’s wrong with this code

```jsx
const fs = require('fs');
const { Command } = require('commander');
const program = new Command();

program
  .name('counter')
  .description('CLI to do file based tasks')
  .version('0.8.0');

program.command('count')
  .description('Count the number of lines in a file')
  .argument('<file>', 'file to count')
  .action((file) => {
    fs.readFile(file, 'utf8', (err, data) => {
      if (err) {
        console.log(err);
      } else {
        const lines = data.split('\n').length;
        console.log(`There are ${lines} lines in ${file}`);
      }
    });
  });

program.parse();
```

![[Screenshot_2024-08-24_at_7.44.51_PM.png]]