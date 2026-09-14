# Core CS & Architecture: Final Deep-Dive Notes

> **How to use this vault note:** This covers the theoretical computer science fundamentals that often pop up in technical MCQs or foundational interview rounds. Read the analogies carefully to lock in the concepts.

---

## 1. Operating Systems (OS)

### A. Processes vs. Threads
*   **Process:** An independent program in execution. It has its own dedicated memory space. (e.g., Opening your web browser).
*   **Thread:** A smaller unit of execution that lives *inside* a process. Multiple threads share the same memory space of their parent process. 
    *   *Plain English Analogy:* A heavy PC game is a **Process**. Inside that game, rendering the graphics is one **Thread**, processing player inputs is a second **Thread**, and playing the background audio is a third. They all work together simultaneously sharing the game's memory.

### B. Memory Management & Virtual Memory
*   **Virtual Memory:** When your computer runs out of physical RAM, the OS creates "fake" RAM by using a section of your slower hard drive (the Swap file/Pagefile). 
*   **Paging:** The OS divides memory into equal-sized blocks called "Pages" and moves them between RAM and the hard drive as needed. 
    *   *Performance Note:* Modern Linux setups often use compressed RAM blocks (like `zRAM`) instead of a traditional hard drive swap file. It compresses inactive pages directly in memory, which is significantly faster than writing to a physical disk.

### C. Deadlocks
*   **Concept:** A situation where two or more processes are unable to proceed because each is waiting for the other to release a resource.
*   *Analogy:* Two cars meet at a narrow, one-lane bridge from opposite directions. Neither can move forward until the other backs up, but neither is willing to back up. The system freezes.

---

## 2. Computer Networks (CN)

### A. TCP vs. UDP (Transport Layer Protocols)
*   **TCP (Transmission Control Protocol):** Reliable, but slower. It requires a "3-way handshake" to establish a connection and guarantees every single packet of data arrives in the correct order. 
    *   *Use case:* Loading a webpage or downloading a file. You cannot afford to have random chunks of a PDF missing.
*   **UDP (User Datagram Protocol):** Fast, but unreliable. It just fires data packets at the receiver without checking if they arrived.
    *   *Use case:* Live video streaming or tactical multiplayer shooters. In a fast-paced game, getting the absolute newest player coordinate data instantly is more important than waiting to recover a dropped packet from 2 seconds ago.

### B. IPv4 vs. IPv6
*   **IPv4:** The older standard. Uses 32-bit addresses (e.g., `192.168.1.1`). Because we ran out of IPv4 addresses, routers have to use NAT (Network Address Translation) to hide multiple devices behind one public IP, which slows down routing.
*   **IPv6:** The modern standard. Uses 128-bit addresses. It provides so many unique addresses that every device gets its own public IP, allowing for direct end-to-end connections and simplifying router configuration to bypass artificial network bottlenecks.

### C. The 7-Layer OSI Model (Top to Bottom)
1.  **Application:** What you see (HTTP, Web Browser).
2.  **Presentation:** Data translation and encryption (SSL/TLS).
3.  **Session:** Establishing and terminating connections.
4.  **Transport:** TCP/UDP (Data delivery).
5.  **Network:** IP Addresses and Routing (Moving data across the internet).
6.  **Data Link:** MAC Addresses (Moving data between local devices like a switch).
7.  **Physical:** The actual cables, Wi-Fi radio waves, and Ethernet pulses.

---

## 3. REST API Fundamentals

### A. HTTP Methods (CRUD Operations)
*   **`GET` (Read):** Retrieves data. (e.g., Loading a user's profile). Should never modify the database.
*   **`POST` (Create):** Submits new data to the server. (e.g., Submitting a registration form).
*   **`PUT` (Update - Full):** Replaces an entire existing resource. (e.g., Overwriting a user's entire profile).
*   **`PATCH` (Update - Partial):** Modifies only a specific part of a resource. (e.g., Changing just the user's password).
*   **`DELETE` (Delete):** Removes a resource from the server.

### B. Crucial HTTP Status Codes
*   **`200 OK` / `201 Created`:** Success! The request worked perfectly.
*   **`400 Bad Request`:** The client (frontend) messed up. (e.g., Zod validation failed because the user sent malformed data).
*   **`401 Unauthorized`:** The user needs to log in (Missing or invalid JWT).
*   **`403 Forbidden`:** The user is logged in, but lacks the proper RBAC permissions (e.g., a Student trying to access an Admin route).
*   **`404 Not Found`:** The requested URL or resource does not exist.
*   **`500 Internal Server Error`:** The server (backend) crashed. The code threw an unhandled exception.

---

## 4. Version Control (Git)

### A. Basic Workflow
1.  **`git clone`:** Downloads a repository to your local machine.
2.  **`git add .`:** Stages all modified files, preparing them for a commit.
3.  **`git commit -m "Message"`:** Takes a snapshot of the staged changes and saves them locally.
4.  **`git push`:** Uploads your local commits to the remote repository (GitHub).
5.  **`git pull`:** Downloads the newest changes from GitHub and merges them into your local files.

### B. Merge Conflicts
*   **What it is:** When two developers edit the *exact same line* of code in a file and try to merge their changes together. Git panics because it doesn't know whose code to keep.
*   **How to fix it:** Git will pause the merge and mark the file. You must open the file in your code editor, manually choose which lines to keep (Accept Current, Accept Incoming, or Accept Both), save the file, and make a new commit to resolve the conflict.