# Backend - FastAPI with PostgreSQL

This directory contains the backend of the application built with FastAPI and a PostgreSQL database.

## Prerequisites

- Python 3.8 or higher
- Poetry (for dependency management)
- PostgreSQL (ensure the database server is running)
- Environment variables set up

### Installing Poetry

To install Poetry, follow these steps:

```sh
curl -sSL https://install.python-poetry.org | python3 -
```

Add Poetry to your PATH (if not automatically added):

## Setup Instructions

1. **Clone the Repository and Navigate to the backend directory**:
    ```sh
    git clone <your-repository-url>
    cd <repository-directory>/backend
    ```

2. Set up a virtual environment (optional but recommended)
    ```sh
    python -m venv venv
    source venv/bin/activate
    ```
3. **Install dependencies using Poetry**:
    ```sh
    poetry install --no-dev --no-interaction
    ```

4. **Set up the database with the necessary tables**:
    ```sh
    poetry run bash ./prestart.sh
    ```

5. **Run the backend server**:
    ```sh
    poetry run uvicorn app.main:app --reload --port 8000
    ```

6. **Update configuration**:
   Ensure you update the necessary configurations in the `.env` file, particularly the database configuration.

7. Open your browser and navigate to `http://<your-ip-address>:8000/redoc` to access the FastAPI documentation.
!['/redoc endpoint'](../images/redoc-endpoint.png)


### Using Docker
This guide provides instructions on how to run the backend application using Docker.

**Prerequisites**
- Docker installed on your machine.
- A PostgreSQL database running. You can run PostgreSQL using Docker or have it installed on your local machine.

**Steps**
**Step 1: Build the Docker Image**
To build the Docker image for the backend application, navigate from your project root to the directory containing the Dockerfile and run:
```sh
cd ./backend/
docker build -t backend-app .
```

**Step 2: Run PostgreSQL Database**
If you don't have PostgreSQL installed locally, you can run it using Docker. Run the following command to start a PostgreSQL container:
```sh
docker run -d \
  --name postgres \
  -e POSTGRES_USER=your_db_user \
  -e POSTGRES_PASSWORD=your_db_password \
  -e POSTGRES_DB=your_db_name \
  -p 5432:5432 \
  postgres:16.3
```
Make sure to replace your_db_user, your_db_password, and your_db_name with your actual database credentials.

**Step 3: Set Environment Variables**
Create a .env file in the directory containing the Dockerfile with the following content, or set these environment variables directly in your terminal:
```env
DATABASE_URL=postgresql://your_db_user:your_db_password@localhost/your_db_name
```

**Step 4: Run the Backend Application**
Finally, run the backend application container:
```sh
docker run -d \
  --name backend-app \
  --env-file .env \
  -p 8000:8000 \
  backend-app
```
This command starts the backend application and maps port 8000 of your host machine to port 8000 of the Docker container.

**Step 5: Verify the Backend Application**
Open your browser and navigate to `http://<your-ip-address>:8000/docs` to access the FastAPI documentation.
!['/docs endpoint'](../images/docs-endpoint.png)
