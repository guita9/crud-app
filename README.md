# FastAPI CRUD Application with PostgreSQL and Dev Containers

This project demonstrates a simple yet robust setup for a web application built with **FastAPI** (a Python web framework), a **PostgreSQL** database, and a smooth development experience using **VS Code Dev Containers**. It includes basic Create, Read, Update, and Delete (CRUD) operations for managing user data.

---

## 📚 Table of Contents

- [Project Overview](#project-overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Cloning the Repository](#cloning-the-repository)
  - [Core Components Explained](#core-components-explained)
  - [Opening in VS Code Dev Containers](#opening-in-vs-code-dev-containers)
  - [Accessing the Application](#accessing-the-application)
- [Application Endpoints (API Usage)](#application-endpoints-api-usage)
- [Project Structure](#project-structure)
- [Future Improvements](#future-improvements)
- [Why This Setup? (Thought Process)](#why-this-setup-thought-process)
  - [Choosing Technologies](#choosing-technologies)
  - [Ensuring Consistency (Dev Containers & Docker)](#ensuring-consistency-dev-containers--docker)
  - [Making it Easy to Start](#making-it-easy-to-start)
  - [Database Management](#database-management)

---

## 🚀 Project Overview

A simple web API to manage user information (name, age, email), built with:

- **FastAPI** – Modern Python framework for APIs.
- **PostgreSQL** – Reliable and scalable relational DB.
- **Docker & Docker Compose** – Ensures consistent environments.
- **VS Code Dev Containers** – Seamless development with zero manual setup.

The goal: a production-ready setup that’s easy to develop and deploy.

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
### Core Components Explained

FastAPI Application (app/ directory)

  - main.py: This is the main file for your FastAPI application. It's where the FastAPI instance is created and all the API endpoints (routes) like /users/ for creating, reading, updating, and deleting users are defined. It uses Depends to inject a database session into each endpoint, making sure database operations are handled correctly.

  - models.py: This file defines the structure of your database tables using SQLAlchemy. For example, the User class here describes what a "user" record looks like in your PostgreSQL database (it has an ID, name, age, and email).

  - schemas.py: This file uses Pydantic to define how data should look when it comes in or goes out of your API. It ensures that the data you receive (e.g., when creating a user) has the correct fields and types, and that the data you send back is well-structured. This helps prevent errors and makes your API predictable.

  - database.py: This file handles the connection to your PostgreSQL database. It sets up the "engine" (the actual connection), a "sessionmaker" (to create individual conversation sessions with the database), and a "Base" for your models. Crucially, it gets the database connection details from an environment variable (DATABASE_URL), which is a good practice for security and flexibility.

Docker Compose (docker-compose.yml)

This file is like a blueprint for running multiple Docker containers together.

  - services: Defines two main services:

  - db: This is your PostgreSQL database. It specifies which PostgreSQL image to use (postgres:15), sets up the database name, username, and password using environment variables, and creates a pgdata volume to store your database's actual data persistently (so you don't lose it when the container stops). It also includes a healthcheck to ensure the database is fully ready before the application tries to connect.

  - app: This is your FastAPI application. It tells Docker to build this service using your Dockerfile in the current directory (build: .). It depends_on the db service, meaning it won't start until the database is healthy. It also sets the DATABASE_URL environment variable for your application to connect to the db service. ports: "8000:8000" means that port 8000 inside the container is made available on port 8000 of your local machine. The volumes: .:/app line is important for development: it synchronizes your local project folder with the /app folder inside the container, so any changes you make to your code on your computer are immediately reflected in the running application inside the container.

Dockerfile

This file contains a set of instructions for building the image for your FastAPI application. Think of an image as a lightweight, standalone, executable package that includes everything needed to run a piece of software.
``` bash 
    FROM python:3.11-slim: Starts with a slim version of Python 3.11. "Slim" images are smaller and more secure because they contain only the essential components, which is great for production.

    WORKDIR /app: Sets the working directory inside the container to /app. All subsequent commands will run from here.

    COPY requirements.txt .: Copies your requirements.txt file (which lists all your Python library dependencies) into the container.

    RUN pip install --no-cache-dir -r requirements.txt: Installs all the Python libraries listed in requirements.txt. --no-cache-dir helps reduce the final image size by not storing installation caches.

    COPY ./app ./app: Copies your actual FastAPI application code (from your local app folder) into the /app directory inside the container.

    CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]: This is the command that gets executed when the Docker container starts. It runs uvicorn, an ASGI server, to serve your FastAPI application (app.main:app) on all available network interfaces (--host 0.0.0.0) and on port 8000.
```
Dev Containers (.devcontainer/devcontainer.json)

This special file tells VS Code how to set up your development environment inside a Docker container.

  - name: Just a friendly name for your development environment.

  - dockerComposeFile & service: Tells VS Code to use your docker-compose.yml file and connect specifically to the app service defined within it. This is how VS Code knows where to run your code.

  - workspaceFolder: Specifies that your project files should appear in the /app folder inside the container.

  - customizations.vscode: Allows you to customize VS Code's behavior specifically for this project:

  - settings: Sets the default terminal shell to /bin/bash inside the container.

  - extensions: Automatically installs useful VS Code extensions like the Python extension (for code completion, debugging, etc.) and the Docker extension (for managing containers from within VS Code).

  - forwardPorts: Automatically forwards port 8000 from the container to your local machine, so you can access your web app via localhost:8000.

  - postCreateCommand: A command that runs after the container is built for the first time. In your case, it automatically installs all your Python dependencies from requirements.txt, so your environment is ready to use without manual steps.

  - remoteEnv: Sets environment variables that are available inside the dev container. Here, DATABASE_URL is set, ensuring your application can connect to the database within the Docker network.

### Opening in VS Code Dev Containers

  - Open VS Code → File > Open Folder → Select the project folder.
  - Click "Reopen in Container"
  - Or use: Ctrl+Shift+P → “Remote-Containers: Reopen in Container”.
  - Wait for the container to build and set up (first time only).
  - You’re now coding inside the container – all dependencies are pre-installed.

### Accessing the Application

Once the container is ready:
  - App auto-starts at http://localhost:8000
  - Swagger UI: http://localhost:8000/docs
  - ReDoc: http://localhost:8000/redoc

## 📡 Application Endpoints (API Usage)

| Method | Path              | Description        | Request Body Example                                           | Response Example                                             |
|--------|-------------------|--------------------|----------------------------------------------------------------|--------------------------------------------------------------|
| POST   | `/users/`         | Create a new user  | `{"name": "Jane Doe", "age": 30, "email": "jane@example.com"}` | `{"id": 1, "name": "Jane Doe", "age": 30, "email": "..."}`   |
| GET    | `/users/`         | Get all users      | None                                                           | `[{"id": 1, "name": "Jane", "age": 30, ...}]`                |
| GET    | `/users/{user_id}`| Get user by ID     | None                                                           | `{"id": 1, "name": "Jane", "age": 30, "email": "..."}`       |
| PUT    | `/users/{user_id}`| Update user by ID  | `{"name": "Jane D.", "age": 31, "email": "jane.d@example.com"}`| `{"id": 1, "name": "Jane D.", "age": 31, "email": "..."}`    |
| DELETE | `/users/{user_id}`| Delete user by ID  | None                                                           | `{"message": "User deleted successfully"}`                   |

> You can test these endpoints using:
> - Swagger UI: [http://localhost:8000/docs](http://localhost:8000/docs)  
> - ReDoc: [http://localhost:8000/redoc](http://localhost:8000/redoc)  
> - Or tools like **Postman** or `curl`


## 🗂 Project Structure
``` bash
fastapi-crud-app/
├── .devcontainer/ # VS Code dev container setup
│ └── devcontainer.json # Dev container configuration
├── app/ # Core FastAPI application
│ ├── database.py # DB connection and session setup
│ ├── main.py # API routes and app instance
│ ├── models.py # SQLAlchemy models
│ └── schemas.py # Pydantic schemas for validation
├── .gitignore # Excludes files from pushing to repo
├── docker-compose.yml # Compose config for app + PostgreSQL
├── Dockerfile # Builds the FastAPI app container
└── requirements.txt # Python dependencies
```

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

