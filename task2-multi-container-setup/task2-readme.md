# MindFuel Quote Delivery Service - Multi-Container Setup

MindFuel is a mental-wellness startup that delivers daily motivational quotes to subscribers via email. This project demonstrates a **multi-container Docker setup** using Docker Compose, including the Python application and a PostgreSQL database.

## Features
- Fetches motivational quotes from ZenQuotes API
- Reads subscribers from a PostgreSQL database
- Sends daily personalized emails at 7 AM
- Logs all events for monitoring
- Fully containerized using Docker Compose

## Requirements
- Docker installed
- `.env` file with credentials

## Folder Structure
```
task2-multi-container-setup/
├── app/
│   ├── config.py
│   ├── email_sender.py
│   ├── db.py
│   ├── logger_config.py
│   ├── main.py
│   ├── quotes.py
│   └── README.md
├── requirements.txt
├── compose.yml
├── Dockerfile
├── task2-readme.md
└── .env
```

## Compose.yml Example
```yaml
version: "3.9"

services:
  db:
    image: postgres:15-alpine
    container_name: postgres_db
    env_file:
      - .env
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: python_app
    env_file:
      - .env
    depends_on:
      - db
    command: python main.py

volumes:
  postgres_data:
```

# Docker Compose Commands Used

All commands below should be executed **from the project root directory**.

---

## Start or Build the Containers

Build the Docker images and start all services (application and database):

```bash
docker compose up -d
```

**output:**
```text
[+] Running 2/2
 ✔ Container postgres_db  Started
 ✔ Container python_app   Started 
```

---

## Check Container Logs

View logs for the application container to confirm successful execution:

```bash
docker compose logs -f app
```

**Sample output:**
```text
python_app   | 2025-12-17 13:58:55,125 - INFO - main - (<module>) - App started at 2025-12-17 13:58:55
python_app   | 2025-12-17 13:58:55,125 - INFO - main - (setup_users) - Starting user setup...
python_app   | 2025-12-17 13:58:55,141 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,143 - INFO - db - (init_db) - Table 'users' created or already exists.
python_app   | 2025-12-17 13:58:55,143 - INFO - db - (init_db) - Table 'email_logs' created or already exists.
python_app   | 2025-12-17 13:58:55,144 - INFO - db - (init_db) - Database initialized successfully.
python_app   | 2025-12-17 13:58:55,158 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,161 - INFO - db - (add_user) - ℹUser 'faruksedik@yahoo.com' already exists. No insertion made.
python_app   | 2025-12-17 13:58:55,164 - INFO - main - (setup_users) - User faruksedik@yahoo.com added successfully.
python_app   | 2025-12-17 13:58:55,178 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,182 - INFO - db - (add_user) - ℹUser 'faruksedik@gmail.com' already exists. No insertion made.
python_app   | 2025-12-17 13:58:55,183 - INFO - main - (setup_users) - User faruksedik@gmail.com added successfully.
python_app   | 2025-12-17 13:58:55,196 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,200 - INFO - db - (add_user) - ℹUser 'sedikjafar@gmail.com' already exists. No insertion made.
python_app   | 2025-12-17 13:58:55,201 - INFO - main - (setup_users) - User sedikjafar@gmail.com added successfully.
python_app   | 2025-12-17 13:58:55,217 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,220 - INFO - db - (add_user) - ℹUser 'matthewtoba5@gmail.com' already exists. No insertion made.
python_app   | 2025-12-17 13:58:55,221 - INFO - main - (setup_users) - User matthewtoba5@gmail.com added successfully.
python_app   | 2025-12-17 13:58:55,233 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:55,236 - INFO - db - (add_user) - ℹUser 'solapeajiboye@gmail.com' already exists. No insertion made.
python_app   | 2025-12-17 13:58:55,237 - INFO - main - (setup_users) - User solapeajiboye@gmail.com added successfully.
python_app   | 2025-12-17 13:58:55,237 - INFO - main - (setup_users) - All users added successfully.
python_app   | 2025-12-17 13:58:55,239 - INFO - main - (setup_users) - User setup completed successfully.
python_app   | 2025-12-17 13:58:55,239 - INFO - main - (run_quote_dispatch) - === Quote Dispatch Workflow Started ===
python_app   | 2025-12-17 13:58:55,239 - INFO - quotes - (fetch_quotes) - Starting fetch from ZenQuotes API (limit=20, timeout=10s)...
python_app   | 2025-12-17 13:58:56,922 - INFO - quotes - (fetch_quotes) - Successfully fetched 20 quotes from ZenQuotes API.
python_app   | 2025-12-17 13:58:56,926 - INFO - main - (run_quote_dispatch) - Fetched 20 quotes successfully from ZenQuotes API.
python_app   | 2025-12-17 13:58:56,928 - INFO - main - (run_quote_dispatch) - Sending daily quotes...
python_app   | 2025-12-17 13:58:56,928 - INFO - email_sender - (send_emails_to_subscribers) - Starting daily email delivery process...
python_app   | 2025-12-17 13:58:56,968 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:56,981 - INFO - db - (get_eligible_subscribers) - Fetched 0 eligible subscribers for frequency 'daily'.
python_app   | 2025-12-17 13:58:56,982 - WARNING - email_sender - (send_emails_to_subscribers) - No active daily subscribers found.
python_app   | 2025-12-17 13:58:56,983 - INFO - main - (run_quote_dispatch) - Sending weekly quotes...
python_app   | 2025-12-17 13:58:56,983 - INFO - email_sender - (send_emails_to_subscribers) - Starting weekly email delivery process...
python_app   | 2025-12-17 13:58:57,024 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:57,038 - INFO - db - (get_eligible_subscribers) - Fetched 0 eligible subscribers for frequency 'weekly'.
python_app   | 2025-12-17 13:58:57,039 - WARNING - email_sender - (send_emails_to_subscribers) - No active weekly subscribers found.
python_app   | 2025-12-17 13:58:57,040 - INFO - main - (run_quote_dispatch) - Sending summary email to admin...
python_app   | 2025-12-17 13:58:57,041 - INFO - email_sender - (send_summary_to_admin) - Generating daily summary report for 2025-12-17...
python_app   | 2025-12-17 13:58:57,081 - INFO - db - (get_conn) - Successfully connected to database 'apidb' on host 'db:5432'.
python_app   | 2025-12-17 13:58:57,091 - INFO - db - (get_logs_for_date) - Retrieved 5 log(s) for date 2025-12-17.
python_app   | 2025-12-17 13:58:57,092 - INFO - email_sender - (send_summary_to_admin) - Fetched 5 log entries for 2025-12-17. Sent: 0, Failed: 5
python_app   | 2025-12-17 13:58:57,102 - INFO - email_sender - (send_summary_to_admin) - Attempting to send summary email to admin: faruksedik@yahoo.com
python_app   | 2025-12-17 13:59:00,786 - INFO - email_sender - (send_summary_to_admin) - Summary email successfully sent to admin faruksedik@yahoo.com
python_app   | 2025-12-17 13:59:00,786 - INFO - main - (run_quote_dispatch) - === Quote Dispatch Workflow Finished ===
python_app   | 2025-12-17 13:59:00,789 - INFO - main - (<module>) - App finished at 2025-12-17 13:59:00 (Runtime: 5.66s)
```

---

## Check Running Containers

```bash
docker ps
```

**Sample output:**
```text
CONTAINER ID   IMAGE                COMMAND                  CREATED       STATUS         PORTS                    NAMES
296dfc176199   postgres:15-alpine   "docker-entrypoint.s…"   6 hours ago   Up 3 minutes   0.0.0.0:5432->5432/tcp   postgres_db
```

---

## Stop the Containers

Stop and remove all containers while preserving persistent volumes (database data is retained):

```bash
docker compose down
```

**Sample output:**
```text
[+] Running 3/3
 ✔ Container python_app                           Removed                                                     0.0s 
 ✔ Container postgres_db                          Removed                                                     0.6s 
 ✔ Network task2-multi-container-setup_default    Removed                                                     0.3s
```

---
