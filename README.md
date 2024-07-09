# Full-Stack FastAPI and React Template

Welcome to the Full-Stack FastAPI and React template repository. This repository serves as a demo application for interns, showcasing how to set up and run a full-stack application with a FastAPI backend and a ReactJS frontend using ChakraUI.


---



This is a full stack web application that consists of a frontend built with Vite and a backend built with Python's FastAPI. The services are containerized using Docker and orchestrated using Docker Compose. 

## Prerequisites

- Docker
- Docker Compose

## Project Structure

The repository is organized into two main directories:

- **frontend**: Contains the ReactJS application.
- **backend**: Contains the FastAPI application and PostgreSQL database integration.

Each directory has its own README file with detailed instructions specific to that part of the application.

```
.
├── backend
│   ├── Dockerfile
│   ├── pyproject.toml
│   ├── poetry.lock
│   ├── .env
│   └── ... (other backend files)
├── frontend
│   ├── Dockerfile
│   ├── package.json
│   ├── package-lock.json
│   ├── vite.config.js
│   ├── nginx.conf
│   └── ... (other frontend files)
├── docker-compose.yml
└── README.md
```

## Getting Started

To get started with this template, please follow the instructions in the respective directories:

- [Frontend README](./frontend/README.md)
- [Backend README](./backend/README.md)

### 1. Clone the Repository

```sh
git clone <repository-url>
cd <repository-directory>
```

### 2. Set Up Environment Variables

#### Backend

Create a `.env` file in the `backend` directory with the following content:

```
POSTGRES_SERVER=db
POSTGRES_PORT=5432
POSTGRES_DB=app
POSTGRES_USER=app
POSTGRES_PASSWORD=changethis123
DOMAIN=yourdomain.com
```

Replace `yourdomain.com` with your actual domain.

### 3. Build and Run the Application

Navigate to the root of the project directory and run the following command:

```sh
docker-compose up --build
```

This command will build the Docker images for the frontend and backend, and start the services defined in the `docker-compose.yml` file.

### 4. Accessing the Services

- **Frontend:** Accessible at `http://<yourdomain.com>`
- **Backend API:** Accessible at `http://<yourdomain.com>/api`
- **Backend Docs (Swagger):** Accessible at `http://<yourdomain.com>/docs`
- **Backend Docs (Redoc):** Accessible at `http://<yourdomain.com>/redoc`
- **Database Adminer:** Accessible at `http://db.<yourdomain.com>`
- **Traefik Dashboard:** Accessible at `http://proxy.<yourdomain.com>`

## Frontend Service

The frontend is built using Vite and served using Nginx.

### Dockerfile

```Dockerfile
# frontend/Dockerfile
FROM node:20-alpine as build

# Set the working directory
WORKDIR /app

# Copy the package.json and package-lock.json
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the application code
COPY . .


# Build the application
RUN npm run build

# Production image
FROM nginx:stable-alpine

# Remove default NGINX configuration
RUN rm -rf /etc/nginx/conf.d

# Copy custom NGINX configuration
COPY nginx.conf /etc/nginx/nginx.conf

# Copy the build output to the Nginx HTML folder
COPY --from=build /app/dist /usr/share/nginx/html

# Expose the port the app runs on
EXPOSE 80

# Command to run Nginx
CMD ["nginx", "-g", "daemon off;"]
```

## Backend Service

The backend is built using FastAPI and managed with Poetry for dependency management.

### Dockerfile

```Dockerfile
# backend/Dockerfile
FROM python:3.10-slim

# Set the working directory in the container
WORKDIR /app

# Copy the dependency files to the working directory
COPY pyproject.toml poetry.lock ./

# Install Poetry
RUN pip install poetry

# Install dependencies
RUN poetry install --no-dev

# Copy the rest of the application code
COPY . .

# Expose the port that your application runs on
EXPOSE 8000

# Convert line endings of prestart.sh to Unix format
RUN apt-get update && apt-get install -y dos2unix \
    && dos2unix ./prestart.sh \
    && chmod +x ./prestart.sh

# Run the database initialization script
ENV PYTHONPATH=/app

# Command to run the application
CMD ["bash", "-c", "poetry run bash ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]
```

## Docker Compose Configuration

The services are defined in the `docker-compose.yml` 


  
## Notes

- Ensure that you replace `yourdomain.com` with your actual domain in the `.env` file and the Docker Compose labels.
- The environment variable `DOMAIN` should be consistent across your services for Traefik routing to work correctly.
- The environment variable `EMAIL` should be set.

## Troubleshooting

If you encounter any issues while setting up or running the application, please refer to the Docker and Docker Compose documentation or check the logs for each service using the following command:

```sh
docker-compose logs <service_name>
```

Replace `<service_name>` with the name of the service you want to check (e.g., `frontend`, `backend`, `db`, `adminer`, `traefik`).

---
