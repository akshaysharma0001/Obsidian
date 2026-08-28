- By using commander package in node
```javascript
import fs from 'fs'
import { Command } from 'commander'
const program=new Command() 

program
    .name('txt-op')
    .description('This cli program can perform several operations on a txt file')

program.command('count')
    .description('Count number of words in a file')
    .argument('<string>','File name')
    .action((file)=>{
        fs.readFile(file,"utf-8",(err,data)=>{
            if(err){
                console.log(err)
            }
            else{
               let count=0;
                for(let i =0;i<data.length;i++){
                    if(data[i]==" "){
                        count++
                    }
                }
                console.log("Number of words : "+ count)
            }
        })
    })
    program.parse();
```