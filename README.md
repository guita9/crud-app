FastAPI CRUD Application with PostgreSQL and Dev Containers

This project demonstrates a simple yet robust setup for a web application built with FastAPI (a Python web framework), a PostgreSQL database, and designed for a smooth development experience using VS Code Dev Containers. It includes basic Create, Read, Update, and Delete (CRUD) operations for managing user data.

Table of Contents

    Project Overview

    Why This Setup? (Thought Process)

        Choosing Technologies

        Ensuring Consistency (Dev Containers & Docker)

        Making it Easy to Start

        Database Management

    Getting Started

        Prerequisites

        Cloning the Repository

        Opening in VS Code Dev Containers

        Accessing the Application

    Application Endpoints (API Usage)

    Project Structure

    Core Components Explained

        FastAPI Application (app/ directory)

        Docker Compose (docker-compose.yml)

        Dockerfile

        Dev Containers (.devcontainer/devcontainer.json)

    Future Improvements

1. Project Overview

This project is a simple web API that allows you to manage user information (like name, age, and email). It's built with:

    FastAPI: A modern, fast (high-performance) web framework for building APIs with Python.

    PostgreSQL: A powerful and reliable open-source relational database for storing our user data.

    Docker & Docker Compose: Tools that help package our application and its database into isolated environments (containers), making them easy to run anywhere consistently.

    VS Code Dev Containers: A feature in VS Code that allows you to develop inside a consistent and pre-configured environment, ensuring everyone on the team has the same setup.

The goal was to create a production-ready setup that's also easy to develop and deploy.

2. Why This Setup? (Thought Process)

When approaching this task, my primary goals were to create a reliable, reproducible, and easy-to-use development and deployment setup. Here's a breakdown of the decisions and the reasoning behind them:

Choosing Technologies

    Python & FastAPI: The task allowed for Python, NodeJS, or Go. I chose Python due to its widespread adoption, rich ecosystem, and my familiarity, which allows for faster development. FastAPI was a natural choice because it's built on modern Python features, offers excellent performance, automatic API documentation (Swagger UI/ReDoc), and strong type checking, which helps catch errors early.

    PostgreSQL: The task specifically preferred PostgreSQL. It's a robust, feature-rich, and highly scalable database, suitable for production environments.

Ensuring Consistency (Dev Containers & Docker)

One of the biggest challenges in team development is ensuring everyone's development environment is identical. Differences in operating systems, Python versions, or installed libraries can lead to "it works on my machine" problems.

    The Problem: Without a consistent environment, developers might spend a lot of time troubleshooting setup issues rather than writing code.

    The Solution: Docker & Dev Containers.

        Docker allows us to package our application and its dependencies (like Python, specific libraries) into a self-contained unit called an "image." This image can then be run as a "container" on any machine with Docker installed, guaranteeing the same environment every time.

        Docker Compose helps us manage multiple related containers (our FastAPI app and our PostgreSQL database) as a single unit, defining how they interact and share networks.

        VS Code Dev Containers leverages Docker Compose. Instead of installing Python, FastAPI, and PostgreSQL directly on your computer, VS Code connects into a running Docker container (our app service). This means your local machine only needs Docker and VS Code – everything else is managed inside the container. This eliminates setup headaches and ensures that what works in development will work when deployed.

Making it Easy to Start

    postCreateCommand in devcontainer.json: To make the developer's life even easier, I added  "postCreateCommand": "pip install -r requirements.txt" [cite: devcontainer.json]. This command automatically runs inside the development container the first time it's created. This means you don't even have to manually install Python packages; the environment is ready to code as soon as you open the project in VS Code.

    forwardPorts: We forward port 8000 from the container to your local machine [cite: devcontainer.json]. This means you can access your running application in your web browser at http://localhost:8000, just as if it were running directly on your computer.

Database Management

    Separate db service: By defining the PostgreSQL database as a separate service (db) in docker-compose.yml [cite: docker-compose.yml], we achieve better modularity. The application doesn't need to know how the database runs, only how to connect to it.

    DATABASE_URL environment variable: Rather than hardcoding database credentials, the connection string (DATABASE_URL) is passed as an environment variable [cite: devcontainer.json, docker-compose.yml]. This is a standard and secure practice, allowing different configurations for development, testing, and production without changing code.

    Database Health Check: In docker-compose.yml, a healthcheck is added to the db service [cite: docker-compose.yml]. This tells Docker Compose to wait until the PostgreSQL database is fully ready and accepting connections before starting the app service. This prevents the application from trying to connect to a database that isn't yet available, avoiding startup errors.

    Volume for Database Data: The pgdata volume is used to persist the database data [cite: docker-compose.yml]. This means if you stop and restart your Docker containers, your database data won't be lost.

3. Getting Started

Follow these steps to get the project up and running on your machine.

Prerequisites

You need the following software installed on your computer:

    Git: For cloning the repository.

    Docker Desktop: This includes Docker Engine and Docker Compose.

    Visual Studio Code (VS Code): Your code editor.

    VS Code Remote - Containers extension: Install this extension within VS Code.
