# Full-Stack FastAPI and React Template

Welcome to the Full-Stack FastAPI and React template repository. This repository serves as a demo application for interns, showcasing how to set up and run a full-stack application with a FastAPI backend and a ReactJS frontend using ChakraUI, all managed and proxied efficiently by Traefik.

![Traefik dashboard](./images/image.png)

## Project Structure

The repository is organized into two main directories:

- **frontend**: Contains the ReactJS application.
- **backend**: Contains the FastAPI application and PostgreSQL database integration.

Each directory has its own README file with detailed instructions specific to that part of the application.

## Getting Started

To get started with this template, please follow the instructions in the respective directories:

- [Frontend README](./frontend/README.md)
- [Backend README](./backend/README.md)


## Manual Setup Instructions for Backend and Frontend Applications
### Prerequisites
- Node.js installed (version 14.x or higher)
- npm installed (version 6.x or higher)
- Python 3.8 or higher installed
- Poetry (for dependency management)
- PostgreSQL database running and accessible
- Virtual Machine with a public IP
- Required environment variables set up

### Backend Application
**Step 1: Set Up PostgreSQL**
1. Install PostgreSQL:
```sh
sudo apt update
sudo apt install postgresql postgresql-contrib
```
2. Start PostgreSQL service:
```sh
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
3. Create a new user and database for the application:
```sh
sudo -i -u postgres
psql
CREATE DATABASE your_db_name;
CREATE USER your_user WITH ENCRYPTED PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE your_db_name TO your_user;
\q
exit
```
**Step 2: Set Up Backend Application**
1. Clone the repository and navigate to the backend directory:
```sh
git clone <your-repository-url>
cd <repository-directory>/backend
```

2. Install dependencies using Poetry:
```sh
sudo apt install curl
curl -sSL https://install.python-poetry.org | python3 -
export PATH="$HOME/.local/bin:$PATH"
poetry config virtualenvs.create false
poetry install --no-dev --no-interaction
```
3. Create a .env file in the backend directory and add the following environment variables:
```sh
DATABASE_URL=postgresql://your_user:your_password@localhost/your_db_name
```

4. Run the prestart script and start the backend server:
```sh
chmod +x prestart.sh
poetry run bash ./prestart.sh
poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000
```

5. Verify backend is running:
Open a browser and navigate to `http://<your-public-ip-address>:8000`

### Frontend Application
**Step 3: Set Up Frontend Application**
1. Navigate to the frontend directory:
```sh
cd <repository-directory>/frontend
```
2. Install dependencies using npm:
```sh
npm install
```
3. Build the application:
```sh
npm run build
```
4. Set Up Nginx:
```sh
sudo apt update
sudo apt install nginx
```
5. Configure Nginx:
Create an Nginx configuration file /etc/nginx/sites-available/frontend with the following content:
```nginx
server {
  listen 80;

  location / {
    root /usr/share/nginx/html;
    index index.html index.htm;
    try_files $uri /index.html =404;
  }
}
```
6. Link the Nginx configuration file:
```sh
sudo ln -s /etc/nginx/sites-available/frontend /etc/nginx/sites-enabled/
```
7. Copy the built files to the Nginx root directory:
```sh
sudo mkdir -p /usr/share/nginx/html
sudo cp -r dist/* /usr/share/nginx/html/
```
8. Restart Nginx:
```sh
sudo systemctl restart nginx
```
9. Verify frontend is running:
Open a browser and navigate to `http://<your-public-ip-address>:80`.

### Adminer for Database Management
**Step 4: Set Up Adminer**
1. Install Adminer:
```sh
sudo apt install adminer
sudo systemctl restart apache2
```
2. Enable Adminer configuration:
```sh
sudo ln -s /etc/adminer/apache.conf /etc/apache2/conf-available/adminer.conf
sudo a2enconf adminer
sudo systemctl reload apache2
```
3. Access Adminer:
Open a browser and navigate to `http://<your-public-ip-address>/adminer`.

By following these steps, you should have the backend, frontend, and Adminer services running on your virtual machine. Here are the endpoints for each service:

- Frontend: `http://<your-public-ip-address>:80`
- Backend: `http://<your-public-ip-address>:8000`
- Adminer: `http://<your-public-ip-address>:/adminer`

Ensure your environment variables and configuration files are correctly set up, and your PostgreSQL database is properly configured and accessible.


## Deployment Guide using Docker Compose
This guide will walk you through deploying a multi-service application using Docker Compose on a virtual machine. The application includes a frontend, backend, PostgreSQL database, Adminer, and Traefik as the reverse proxy. The following steps assume you have Docker and Docker Compose installed on your VM.

### Prerequisites
- Docker installed on your virtual machine.
- Docker Compose installed on your virtual machine.
- Basic knowledge of Docker and Docker Compose.

### Step-by-Step Deployment
1. Clone the repository or upload your project files to the VM:
```sh
git clone https://github.com/rotimiAbiola/full-stack-fastapi-project.git
cd <project-root>
```

2. Environment Variables:
Make sure you have the `.env` file at the root of your project directory with the necessary environment variables. Example:
```makefile
DOMAIN=yourdomain.com
```
3. Navigate to the project root to build and deploy with docker-compose:
```sh
docker-compose up --build -d
```
