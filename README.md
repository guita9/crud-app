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
- [Remote - Containers Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

### Cloning the Repository

```bash
git clone <your-github-repo-url>
cd fastapi-crud-app
