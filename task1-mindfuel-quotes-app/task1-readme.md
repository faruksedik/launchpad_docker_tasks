# MindFuel Quote Delivery Service

MindFuel is a mental-wellness startup that delivers daily motivational quotes to subscribers via email. This project containerizes the Python backend service using Docker for reliable deployment.

## Features
- Fetches motivational quotes from ZenQuotes API
- Reads subscribers from a database
- Sends daily personalized emails at 7 AM
- Logs all events for monitoring

## Requirements
- Docker installed
- Local Postgres database
- `.env` file with credentials

## Project Structure
```
task1-mindfuel-quotes-app/
├── app/
│   ├── config.py
│   ├── email_sender.py
│   ├── db.py
│   ├── logger_config.py
│   ├── main.py
│   ├── quotes.py
│   └── README.md
├── requirements.txt
├── Dockerfile
├── task1-readme.md
└── .env
```
## Dockerfile Example
```dockerfile
FROM python:3.11-slim

# Set working directory
WORKDIR /app

# Install dependencies first (layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app/ .

# Run the application
CMD ["python", "main.py"]
```

## Setup Instructions

### 1. Pull the Docker Image from Docker Hub
```bash
docker pull faruksedik/mindfuel-quotes-app:v1
```

### 2. Setup Local Postgres Database
1. Make sure Postgres is installed and running.
2. Create a database for MindFuel (replace with your preferred name):
```bash
psql -U postgres -h localhost -p 5432 -c "CREATE DATABASE mindfuel_db_for_docker;"
```
3. Make sure you know your Postgres username and password (default is `postgres`).

### 3. Configure the `.env` File
Create a `.env` file in your project directory with the following content, updating values as needed:
```bash
PG_HOST=host.docker.internal
PG_PORT=5432
PG_DB=mindfuel_db_for_docker
PG_USER=postgres
PG_PASSWORD=password

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASSWORD=your_email_password
```
> Note: `host.docker.internal` allows the container to connect to your local Postgres database on Windows & Mac. On Linux, you may need to use your host IP.

### 4. Run the Container
```bash
docker run --env-file .env -it faruksedik/mindfuel-quotes-app:v1
```
- The container will fetch quotes, connect to your local database, and send emails according to your configuration.

## Notes
- Ensure your local Postgres database is running before starting the container.
- For production, consider using a managed Postgres database and secure your SMTP credentials properly.
- You can verify database connectivity by checking logs or query