# CodeCraftHub

A simple REST API to track your personal learning goals and courses.

## Overview

CodeCraftHub helps developers keep track of courses they want to learn. Built with Node.js and Express, the platform provides a straightforward REST API for managing your learning journey.

## Features

- Full CRUD operations for course management
- JSON file-based storage (no database needed)
- RESTful API design
- Proper error handling with consistent JSON responses
- Input validation (required fields and allowed status values)
- Bonus endpoints: course statistics and search

## Installation

1. Clone or download the project.
2. Open a terminal in the project folder (`codecrafthub`).
3. Install the Node.js dependencies:

```bash
npm install
```

## Running the Application

Start the Express server:

```bash
npm start
```

For development, you can use nodemon, which restarts the server automatically whenever you save `app.js`:

```bash
npm run dev
```

The API will be available at `http://localhost:5000`.

## Data Structure

Each course is stored in `courses.json` (created automatically on first run) in this format:

```json
{
  "id": 1,
  "name": "Python Basics",
  "description": "Learn Python fundamentals",
  "target_date": "2025-12-31",
  "status": "Not Started",
  "created_at": "2025-11-04 10:30:00"
}
```

Valid `status` values: `Not Started`, `In Progress`, `Completed`.

## API Endpoints

### 1. Add a Course

**POST** `/api/courses`

Request body:

```json
{
  "name": "Python Basics",
  "description": "Learn Python fundamentals",
  "target_date": "2025-12-31",
  "status": "Not Started"
}
```

Success response (201):

```json
{
  "success": true,
  "message": "Course added successfully",
  "course": {
    "id": 1,
    "name": "Python Basics",
    "description": "Learn Python fundamentals",
    "target_date": "2025-12-31",
    "status": "Not Started",
    "created_at": "2025-11-04 10:30:00"
  }
}
```

### 2. Get All Courses

**GET** `/api/courses`

Success response (200):

```json
{
  "success": true,
  "count": 1,
  "courses": [ ... ]
}
```

### 3. Get a Specific Course

**GET** `/api/courses/<id>`

Example: `http://localhost:5000/api/courses/1`

### 4. Update a Course

**PUT** `/api/courses/<id>`

Request body (all fields optional):

```json
{
  "status": "In Progress"
}
```

### 5. Delete a Course

**DELETE** `/api/courses/<id>`

### Bonus Endpoints

**GET** `/api/courses/stats` returns the total number of courses and a count for each status.

```json
{
  "success": true,
  "total": 1,
  "by_status": { "Not Started": 1, "In Progress": 0, "Completed": 0 }
}
```

**GET** `/api/courses/search?q=term` returns courses whose name or description contains the search term.

Example: `http://localhost:5000/api/courses/search?q=python`

### Error Responses

All errors use the same format, with an appropriate status code (400, 404, or 500):

```json
{
  "success": false,
  "error": "Course with ID 99 not found"
}
```

## Testing

GET endpoints can be tested directly in a browser, for example `http://localhost:5000/api/courses`.

For POST, PUT, and DELETE, use a tool such as Postman, the VS Code REST Client extension, or the PowerShell commands below.

**Add a course:**

```powershell
Invoke-RestMethod -Method Post -Uri http://localhost:5000/api/courses -ContentType "application/json" -Body '{"name":"Python Basics","description":"Learn Python fundamentals","target_date":"2025-12-31","status":"Not Started"}'
```

**Update a course:**

```powershell
Invoke-RestMethod -Method Put -Uri http://localhost:5000/api/courses/1 -ContentType "application/json" -Body '{"status":"In Progress"}'
```

**Delete a course:**

```powershell
Invoke-RestMethod -Method Delete -Uri http://localhost:5000/api/courses/1
```

If you use macOS or Linux, the equivalent curl command to add a course is:

```bash
curl -X POST http://localhost:5000/api/courses \
  -H "Content-Type: application/json" \
  -d '{"name":"Python Basics","description":"Learn Python fundamentals","target_date":"2025-12-31","status":"Not Started"}'
```

## Troubleshooting

**Problem:** "Cannot find module 'express'"
**Solution:** Run `npm install` in the project folder.

**Problem:** "Port already in use"
**Solution:** Stop other applications using port 5000, or change the `PORT` value in `app.js`.

**Problem:** `npm : File ... npm.ps1 cannot be loaded because running scripts is disabled on this system` (Windows PowerShell)
**Solution:** Run `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned`, then open a new terminal. Alternatively, use Command Prompt instead of PowerShell.

**Problem:** `npm error ENOENT ... Could not read package.json`
**Solution:** The terminal is in the wrong folder. Navigate to the `codecrafthub` folder first, or open the folder in VS Code and use its built-in terminal.

**Problem:** Changes to `app.js` don't seem to take effect, or `/api/courses/search` returns "Course with ID NaN not found"
**Solution:** The server is running an old version of the code. Stop it with Ctrl + C and start it again, or use `npm run dev` so it restarts automatically. Also make sure `/stats` and `/search` are defined above `/api/courses/:id` in `app.js`.

## Project Structure

```plaintext
codecrafthub/
├── app.js           # Main Express application
├── courses.json     # Data storage (auto-created)
├── package.json     # Dependencies
└── README.md        # Project documentation
```
