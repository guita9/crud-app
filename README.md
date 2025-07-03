# FastAPI CRUD Application with PostgreSQL and Dev Containers

This project demonstrates a simple yet robust setup for a web application built with **FastAPI** (a Python web framework), a **PostgreSQL** database, and a smooth development experience using **VS Code Dev Containers**. It includes basic Create, Read, Update, and Delete (CRUD) operations for managing user data.

---

## 📚 Table of Contents

- [Project Overview](#project-overview)
- [Why This Setup? (Thought Process)](#why-this-setup-thought-process)
  - [Choosing Technologies](#choosing-technologies)
  - [Ensuring Consistency (Dev Containers & Docker)](#ensuring-consistency-dev-containers--docker)
  - [Making it Easy to Start](#making-it-easy-to-start)
  - [Database Management](#database-management)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Cloning the Repository](#cloning-the-repository)
  - [Opening in VS Code Dev Containers](#opening-in-vs-code-dev-containers)
  - [Accessing the Application](#accessing-the-application)
- [Application Endpoints (API Usage)](#application-endpoints-api-usage)
- [Project Structure](#project-structure)
- [Core Components Explained](#core-components-explained)
- [Future Improvements](#future-improvements)

---

## 🚀 Project Overview

A simple web API to manage user information (name, age, email), built with:

- **FastAPI** – Modern Python framework for APIs.
- **PostgreSQL** – Reliable and scalable relational DB.
- **Docker & Docker Compose** – Ensures consistent environments.
- **VS Code Dev Containers** – Seamless development with zero manual setup.

The goal: a production-ready setup that’s easy to develop and deploy.

---

## 🤔 Why This Setup? (Thought Process)

### Choosing Technologies

- **FastAPI**: Fast, type-safe, async-ready, auto-generates docs.
- **PostgreSQL**: Feature-rich and reliable database.

### Ensuring Consistency (Dev Containers & Docker)

To eliminate “it works on my machine” problems:

- **Docker**: Packages everything into an image.
- **Docker Compose**: Manages app + db containers together.
- **VS Code Dev Containers**:  leverages Docker Compose. Instead of installing Python, FastAPI, and PostgreSQL directly on your computer, VS Code connects into a running Docker container (our app service). This means your local machine only needs Docker and VS Code – everything else is managed inside the container. This eliminates setup headaches and ensures that what works in development will work when deployed.

### Making it Easy to Start

- `postCreateCommand`: In devcontainer.json file, to make the developer's life even easier, I added  "postCreateCommand": "pip install -r requirements.txt". This command automatically runs inside the development container the first time it's created. This means you don't even have to manually install Python packages; the environment is ready to code as soon as you open the project in VS Code.
- `forwardPorts`: Forwards port `8000` from the dev container to our local machine so app is available at [http://localhost:8000](http://localhost:8000).

### Database Management

- PostgreSQL runs as a separate service or container.
- Uses environment variables (`DATABASE_URL`) for clean configuration.
- Includes DB health checks & persistent volumes.

---

## 🛠 Getting Started

### Prerequisites

Make sure you have the following installed:

- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Visual Studio Code](https://code.visualstudio.com/)
- [Dev - Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Cloning the Repository

```bash
git clone https://github.com/guita9/crud-app.git
cd fastapi-crud-app
```

### Opening in VS Code Dev Containers

  - Open VS Code → File > Open Folder → Select the project folder.
  - Click "Reopen in Container"
  - Or use: Ctrl+Shift+P → “Remote-Containers: Reopen in Container”.
  - Wait for the container to build and set up (first time only).
  - You’re now coding inside the container – all dependencies are pre-installed.

🌐 Accessing the Application

Once the container is ready:

    App auto-starts at http://localhost:8000

    Swagger UI: http://localhost:8000/docs

    ReDoc: http://localhost:8000/redoc

📡 Application Endpoints (API Usage)
Method	Path	Description	Request Body Example	Response Example
POST	/users/	Create a new user	{"name": "Jane Doe", "age": 30, "email": "jane@example.com"}	{"id": 1, "name": "Jane Doe", "age": 30, "email": "..."}
GET	/users/	Get all users	None	[{"id": 1, "name": "Jane", "age": 30, ...}]
GET	/users/{user_id}	Get user by ID	None	{"id": 1, "name": "Jane", "age": 30, "email": "..."}
PUT	/users/{user_id}	Update user by ID	{"name": "Jane D.", "age": 31, "email": "jane.d@example.com"}	{"id": 1, "name": "Jane D.", "age": 31, "email": "..."}
DELETE	/users/{user_id}	Delete user by ID	None	{"message": "User deleted successfully"}

👉 Test via /docs or tools like Postman, Insomnia, or curl.
🗂 Project Structure

fastapi-crud-app/
├── .devcontainer/
│   └── devcontainer.json         # Dev container setup
├── app/
│   ├── database.py               # DB connection and session
│   ├── main.py                   # API routes
│   ├── models.py                 # SQLAlchemy models
│   └── schemas.py                # Pydantic schemas
├── .dockerignore
├── docker-compose.yml           # Compose config for app + db
├── Dockerfile                   # Builds FastAPI app container
└── requirements.txt             # Python dependencies

🔍 Core Components Explained
app/ Directory

    main.py – FastAPI routes and logic.

    models.py – SQLAlchemy DB model for User.

    schemas.py – Pydantic schemas for request/response validation.

    database.py – DB engine and session via DATABASE_URL.

docker-compose.yml

    db service:

        Uses postgres:15

        Sets up volume pgdata for persistence

        Includes a healthcheck to delay app start until DB is ready

    app service:

        Built via Dockerfile

        Depends on db

        Forwards port 8000

Dockerfile

FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY ./app ./app
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]

.devcontainer/devcontainer.json

VS Code containerized dev environment config:

    Installs Python & Docker extensions.

    Auto-runs pip install -r requirements.txt after first build.

    Forwards port 8000.

    Sets DATABASE_URL for app config.

🔮 Future Improvements

    ✅ Add authentication/authorization

    ✅ Improve error handling and validation

    ✅ Add unit & integration tests

    ✅ Logging & monitoring setup

    ✅ CI/CD via GitHub Actions or Jenkins

    ✅ DB migrations using Alembic

    ✅ Harden Docker security (non-root user, multi-stage builds)

    ✅ Integrate Prometheus/Grafana for observability

