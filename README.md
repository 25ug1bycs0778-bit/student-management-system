# Student Management System

A student directory with a plain HTML/CSS/JavaScript frontend, a Flask REST API, and a local SQLite database. The frontend is served by Flask, so no separate web server or CORS setup is needed.

## Requirements

- Python 3.9 or newer
- pip
- Postman (optional, for API testing)

## Run locally

On Windows, double-click `run.bat` or start it from PowerShell:

```powershell
.\run.bat
```

The launcher creates a virtual environment in `backend\.venv`, installs Flask, starts the API, and opens the app in your browser. Keep the launcher window open while using the system; press Ctrl+C in that window to stop it. Open <http://127.0.0.1:5000> if your browser does not open automatically.

Alternatively, start the app manually from the project directory:

```powershell
cd backend
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python app.py
```

Open <http://127.0.0.1:5000>. Use this address rather than opening the HTML file directly: Flask serves the frontend and API together, and SQLite stores student records at `backend/students.db`.

To use a different local database file, set `STUDENT_DB_PATH` before starting Flask:

```powershell
$env:STUDENT_DB_PATH = "C:\data\students.db"
python app.py
```

## API

All student records use `student_id`, `name`, `email`, `course`, `year` (1–8), and `status` (`Active` or `Inactive`). Student IDs and email addresses must be unique.

| Method | Endpoint | Purpose |
| --- | --- | --- |
| `GET` | `/api/health` | Check that the API is running |
| `GET` | `/api/students` | List records; supports `search`, `status`, `page`, and `per_page` query parameters |
| `GET` | `/api/students/summary` | Counts for total, active, inactive, and distinct courses |
| `POST` | `/api/students` | Create a record |
| `GET` | `/api/students/<id>` | Get one record |
| `PUT` / `PATCH` | `/api/students/<id>` | Replace or update a record |
| `DELETE` | `/api/students/<id>` | Delete a record |

Example create request:

```json
{
  "student_id": "STU-2025-001",
  "name": "Maya Chen",
  "email": "maya.chen@example.edu",
  "course": "Computer Science",
  "year": 2,
  "status": "Active"
}
```

List responses contain `students`, `total`, `page`, and `per_page`. Errors return a JSON `error` message. Duplicate student IDs or email addresses return `409 Conflict`; invalid input returns `400 Bad Request`.

## Test with Postman

Import `postman/Student-Management-System.postman_collection.json` into Postman. Its `baseUrl` variable defaults to `http://127.0.0.1:5000`. Create a student first, then set the `studentId` variable to the returned numeric `id` for get, update, and delete requests.

## Run tests

From `backend`:

```powershell
python -m unittest discover -s tests -v
```
