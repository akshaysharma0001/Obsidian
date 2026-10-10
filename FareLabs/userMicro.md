### MVC(Model View Controller)
>A software design pattern that divides an application into three interconnected parts to separate business logic from the user interface

The Three Components

- **Model**: Manages the data, business rules, logic, and database interactions of the application. 

- **View**: Handles the presentation layer, layout, styling, and visual rendering of data to the user. 

- **Controller**: Acts as an intermediary that receives user input, talks to the Model to process or update data, and selects the appropriate View to render


*process.on('  ' ,    )*
- This snippet registers **event listeners in Node.js** to handle system signals for a graceful shutdown

- **`process.on('SIGTERM', shutdownServer);`**: Listens for a **SIGTERM** (Signal Terminate). This is the generic signal sent by hosting platforms, container orchestrators (like Docker or Kubernetes), or process managers (like PM2) to tell your application to stop.

- **`process.on('SIGINT', shutdownServer);`**: Listens for a **SIGINT** (Signal Interrupt). This is the signal sent when you press **Ctrl + C** in your terminal to manually interrupt a running process.



