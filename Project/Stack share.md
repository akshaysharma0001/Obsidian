# Stack-share 🚀

A full-stack web application built with the MERN stack that allows developers to showcase their project tech stacks, discover new tools, and connect with other developers. 

This project was built to actively apply core web development concepts including RESTful API design, authentication, state management, and robust input validation.

## 🌟 Features

- **Secure Authentication:** User signup and login utilizing JWT (JSON Web Tokens) and password hashing.
- **Robust Validation:** Strict schema validation for all API inputs using **Zod**.
- **Global State Management:** Seamless frontend state handling using **Recoil**.
- **Protected Routes:** Custom authentication middleware to secure backend endpoints and frontend views.
- **Dynamic Filtering:** Query parameter support to search and filter projects by specific technologies (e.g., `?tech=react`).
- **Responsive SPA:** A Single Page Application built with React and React Router for fast, reload-free navigation.

## 🛠️ Tech Stack

**Frontend:**
- React.js
- React Router DOM (Routing)
- Recoil (State Management)
- Custom Hooks (`useFetch`, `useDebounce`)

**Backend:**
- Node.js & Express.js
- MongoDB & Mongoose
- JWT (Authentication)
- Zod (Data Validation)
- CORS & Express Middlewares

## 🚀 Getting Started

Follow these steps to set up the project locally on your machine.

### Prerequisites
Make sure you have the following installed:
- [Node.js](https://nodejs.org/) (v16 or higher)
- [MongoDB](https://www.mongodb.com/) (Local instance or MongoDB Atlas URI)
- [Git](https://git-scm.com/)

📦 DATABASE: Stack-share
│
├─ 📂 COLLECTION: Users
│  ├─ 🔑 _id          [ObjectId]  👉 (Automatically generated unique ID)
│  ├─ 📝 name         [String]    
│  ├─ 📧 email        [String]    👉 (Must be Unique)
│  ├─ 🔒 password     [String]    👉 (Hashed for security)
│  └─ ⏰ createdAt    [Date]      👉 (Timestamp of registration)
│
└─ 📂 COLLECTION: Projects
   ├─ 🔑 _id          [ObjectId]  👉 (Automatically generated unique ID)
   ├─ 🏷️ title        [String]    👉 (e.g., "Real-time Chat App")
   ├─ 📄 description  [String]    👉 (What the project is about)
   ├─ 🛠️ technologies [Array]     👉 (e.g., ["React", "Node.js", "Socket.io"])
   ├─ 🔗 repoLink     [String]    👉 (URL to the GitHub repository)
   ├─ ⏰ createdAt    [Date]      👉 (Timestamp of creation)
   │
   └─ 🔗 RELATIONAL LINK
      └─ 👤 author    [ObjectId]  👉 (References 👉 Users._id)
      

## 📂 Folder Structure

Stack-share/
├── backend/
│   ├── index.js          # Entry point for the Express server
│   ├── db/               # 👈 MongoDB connection and Mongoose models
│   │   ├── db.js         # Connection logic
│   │   └── models.js     # User and Project schemas
│   ├── middleware/       # JWT auth and error handling middlewares
│   ├── routes/           # API route definitions (auth, projects)
│   └── types.js          # Zod schemas for input validation
└── frontend/
    ├── src/
    │   ├── components/   # Reusable UI components (Navbar, ProjectCard)
    │   ├── pages/        # Route components (Home, Login, Dashboard)
    │   ├── store/        # Recoil atoms and selectors
    │   └── hooks/        # Custom React hooks
    └── package.json