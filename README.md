# Two-Tier Flask Application with MySQL (Dockerized)

This is a **Two-Tier Web Application** built with Flask (Frontend/API) and MySQL (Database), fully containerized with Docker.

## Project Structure
- **Frontend:** Flask Application serving HTML templates.
- **Backend:** Python Flask API.
- **Database:** MySQL 5.7.
- **Infrastructure:** Docker Compose for orchestration.

## Prerequisites

Before running the application, ensure you have the following installed:
- [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop/)
- Git (Optional, for cloning)

## Quick Start (Recommended)

The easiest way to run the application is using Docker Compose.

1. **Clone the repository:**
   ```bash
   git clone https://github.com/bangalsubham20/two-tier-flask-app.git
   cd two-tier-flask-app
   ```

2. **Start the application:**
   ```bash
   docker-compose up --build
   ```
   *Note: On Windows, ensure Docker Desktop is running. The `docker-compose.yml` has been optimized for Windows compatibility.*

3. **Access the Application:**
   - **Frontend:** [http://localhost:5000](http://localhost:5000)
   - **Database:** Port `3306` (Exposed for local tools like MySQL Workbench or DBeaver)
     - **Host:** `localhost`
     - **Username:** `root`
     - **Password:** `root`
     - **Database:** `devops`

4. **Stop the Application:**
   Press `Ctrl+C` in the terminal or run:
   ```bash
   docker-compose down
   ```

## Manual Setup (Without Docker Compose)
If you prefer to run containers manually, follow these steps:

1. **Create a Network:**
   ```bash
   docker network create twotier
   ```

2. **Run MySQL Container:**
   ```bash
   docker run -d \
       --name mysql \
       --network=twotier \
       -v mysql-data:/var/lib/mysql \
       -e MYSQL_ROOT_PASSWORD=root \
       -e MYSQL_DATABASE=devops \
       -p 3306:3306 \
       mysql:5.7
   ```

3. **Build and Run Flask App:**
   ```bash
   docker build -t flaskapp .
   docker run -d \
       --name flaskapp \
       --network=twotier \
       -e MYSQL_HOST=mysql \
       -e MYSQL_USER=root \
       -e MYSQL_PASSWORD=root \
       -e MYSQL_DB=devops \
       -p 5000:5000 \
       flaskapp
   ```

## Troubleshooting
- **Database Connection Error:** Ensure the MySQL container is healthy (`docker ps`) before the Flask app starts. Docker Compose handles this with `depends_on`, but manual runs might require a delay.
- **Port Conflicts:** If port `5000` or `3306` is in use, stop other services or modify `docker-compose.yml`.
- **Windows File Sharing:** If volumes fail to mount, ensure you've allowed file sharing for your drive in Docker Desktop settings.
