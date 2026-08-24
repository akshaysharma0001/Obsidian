```javascript
import chalk from 'chalk'
import fs from 'fs'
import {Command} from 'commander'
const data=JSON.parse(fs.readFileSync("todos.json","utf-8"))
let program =new Command()

program
    .name("cli-todo")
    .description("Cli todolist to add delete multiple tasks and store it in json file")

  
program
    .command("show")
    .description("show todolist")
    .action(()=>{
        for(let i =0;i<data.length;i++){
            if(data[i].completed==true){
                console.log((i+1)+ " "+chalk.greenBright(data[i].task))
            }
            else{
                console.log((i+1)+ " "+chalk.redBright(data[i].task))
            }
        }
    })

    program
   .command("add")
    .description("add task")
    .argument('<string>','task name')
    .action((task)=>{


        data[data.length]={"task":task,
            "completed":false
        }
        fs.writeFileSync("todos.json",JSON.stringify(data))
    })


    program
    .command("delete")
    .description("delete task")
    .argument('<integer>','tasknumber')
    .action((num)=>{
        data.splice(num-1,1)
        fs.writeFileSync("todos.json",JSON.stringify(data))
    })

    program
    .command("done")
    .description("task done")
    .argument('<integer>','tasknumber')
    .action((num)=>{
       data[num-1].completed=true
        fs.writeFileSync("todos.json",JSON.stringify(data))
    })

program.parse();
```