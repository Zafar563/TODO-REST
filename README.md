# 📋 Todo REST API & Frontend Application

This project is a ToDo (Task Management) application consisting of an in-memory REST API backend written in **Go (Golang)** and a clean, responsive single-page **HTML/CSS/JS** frontend.

---

## 🛠 Technology Stack

- **Backend:** 
  - Go (version 1.25.0+)
  - **gorilla/mux** (routing library)
  - Go standard packages (`sync`, `net/http`, `json`, etc.)
- **Frontend:**
  - Vanilla HTML5 and CSS3 (modern styling)
  - Pure JavaScript (using ES6 Fetch API to communicate with the backend)

---

## 📂 Project Structure

```text
├── frontend/
│   └── index.html       # User interface (UI) and API integration
├── httpserver/
│   ├── dto.go           # DTO (Data Transfer Object) models and input validation
│   ├── handlers.go      # REST API request handlers
│   └── server.go        # HTTP server configuration, CORS middleware, and routing
├── todo/
│   ├── errors.go        # Custom domain errors (e.g., task not found)
│   ├── list.go          # Thread-safe (sync.RWMutex) in-memory task collection
│   └── task.go          # Task domain model and its methods
├── go.mod               # Go module file
├── go.sum               # Go dependency checksums file
├── main.go              # Main entry point of the application
└── README.md            # Project documentation (this file)
```

---

## ⚙️ Component Overview

### 1. Backend (`todo` and `httpserver`)
- **Thread-Safety:** Tasks are stored in memory inside a map. To handle concurrent client requests safely, a `sync.RWMutex` (Read-Write Mutex) is used in `todo/list.go`.
- **CORS (Cross-Origin Resource Sharing):** A custom `enableCORS` middleware is implemented to allow requests from any origin (`*`) and support all HTTP methods (`GET, POST, PUT, PATCH, DELETE, OPTIONS`), preventing any browser CORS blocking.
- **Request Validation:** When adding a new task, the application validates that the title and description are not empty via `ValidationForCreate` in `dto.go`.

### 2. Frontend (`frontend/index.html`)
- **Single Page Application:** Users can create, read, complete, and delete tasks without reloading the webpage.
- **Aesthetics & UI:** Features a modern layout with clean inputs, hover animations, responsive buttons, and visual state cues (completed tasks are automatically styled with a line-through decoration).

---

## 🚀 Running the Application

### 1. Start the Backend Server:
Navigate to the root directory of the project and execute the following command in your terminal:

```bash
go run main.go
```
Once started, the backend server will listen for requests at `http://localhost:9091`.

### 2. Open the Frontend:
Simply open the `frontend/index.html` file in any modern web browser (e.g., Google Chrome, Mozilla Firefox, Safari).

---

## 📡 REST API Endpoints

Base URL: `http://localhost:9091`

| Method | Endpoint | Description | Request Body (JSON) | Response (JSON / Status) |
| :--- | :--- | :--- | :--- | :--- |
| **POST** | `/tasks` | Create a new task | `{"Title": "...", "Description": "..."}` | `201 Created` + Task DTO |
| **GET** | `/tasks` | Retrieve all tasks | *None* | `200 OK` + List of tasks |
| **GET** | `/tasks/{title}` | Retrieve a task by title | *None* | `200 OK` + Task details |
| **GET** | `/tasks?completed=true` | Retrieve uncompleted tasks | *None* | `200 OK` + Uncompleted tasks |
| **PATCH** | `/tasks/{title}` | Mark a task as completed or uncompleted | `{"Complete": true/false}` | `200 OK` + Updated task |
| **DELETE** | `/tasks/{title}` | Delete a task by title | *None* | `204 No Content` |

> 💡 **Routing Note:** The query endpoint `/tasks?completed=true` is mapped to return uncompleted tasks (`Completed = false`) in the backend routing configuration.

---

## 📌 Development Recommendations

1. **Persistent Storage:** Currently, all tasks are stored `in-memory`. If you restart the Go server, all data will be lost. Adding a database layer such as SQLite or PostgreSQL is recommended for persistence.
2. **Typo Fixes:**
   - In `todo/errors.go`, `ErrTaskAlredyExist` can be renamed to `ErrTaskAlreadyExist`.
   - In `httpserver/dto.go`, the `ErrorDTO` struct field `Masseg` can be renamed to `Message`.
