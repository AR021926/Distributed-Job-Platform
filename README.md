# Distributed Job Processing Platform

A full-stack distributed job processing platform built with **Java, Spring Boot, React, Apache Kafka, MySQL, Redis, JWT Authentication, and Docker**.

The platform allows authenticated users to submit jobs that are asynchronously queued and processed by worker services. Job status and execution results are displayed through a live dashboard.

## Live Demo

**Application:** https://distributed-job-platform-rose.vercel.app

**GitHub:** https://github.com/AR021926/Distributed-Job-Platform

---

## Architecture

```text
                 ┌───────────────────┐
                 │   React Frontend  │
                 │      (Vite)       │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │    API Layer      │
                 │   Spring Boot     │
                 └─────────┬─────────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │    Job Service    │
                 │   Spring Boot     │
                 └────┬────────┬─────┘
                      │        │
              Persist │        │ Publish Job
                      ▼        ▼
                 ┌────────┐  ┌──────────────┐
                 │ MySQL  │  │ Apache Kafka │
                 └────────┘  └──────┬───────┘
                                    │
                                    ▼
                           ┌──────────────────┐
                           │  Worker Service  │
                           │   Spring Boot    │
                           └────────┬─────────┘
                                    │
                                    │ Status / Result
                                    ▼
                           ┌──────────────────┐
                           │   Job Service    │
                           └──────────────────┘

                 Redis → Fast job-status storage
```

### Job Processing Flow

```text
User submits job
      ↓
Job Service
      ↓
Store job in MySQL
      ↓
Publish job event to Kafka
      ↓
Worker Service consumes event
      ↓
Execute job
      ↓
Retry on processing failure
      ↓
Update job status/result
      ↓
Dashboard displays final state
```

---

## Key Features

- Distributed asynchronous job processing
- Apache Kafka based producer-consumer architecture
- Independent worker service
- Job lifecycle tracking
- Job execution result storage
- Retry and failure handling
- JWT-based authentication
- User registration and login
- MySQL persistent storage
- Redis integration for job status
- REST API architecture
- React dashboard
- Dockerized services
- Live cloud deployment
- Shared system-wide job processing history

---

## Job Lifecycle

Jobs move through the processing lifecycle:

```text
SUBMITTED → QUEUED → PROCESSING → COMPLETED
                              ↘ FAILED
```

The dashboard displays job information including:

- Job ID
- Job name
- Current status
- Execution result
- Creation time
- Completion time

---

## Technology Stack

### Backend

- Java
- Spring Boot
- Spring Data JPA
- Spring Security
- REST APIs
- JWT Authentication

### Distributed Processing

- Apache Kafka
- Kafka Producer/Consumer architecture
- Zookeeper
- Worker Service

### Data Layer

- MySQL
- Redis

### Frontend

- React.js
- Vite
- JavaScript
- HTML/CSS

### DevOps & Deployment

- Docker
- Docker Compose
- Git/GitHub
- Railway
- Vercel

---

## Project Structure

```text
Distributed-Job-Platform/
│
├── frontend/
│   └── React frontend application
│
├── job-service/
│   └── Core Spring Boot job and authentication service
│
├── worker-service/
│   └── Kafka consumer and job execution service
│
├── api-gateway/
│   └── API gateway service
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

## Local Architecture

The local development environment can be orchestrated using Docker Compose.

Services defined in `docker-compose.yml` include:

| Service | Purpose |
|---|---|
| Frontend | React user interface |
| API Gateway | API routing layer |
| Job Service | Job management and authentication |
| Worker Service | Asynchronous job processing |
| Kafka | Job message broker |
| Zookeeper | Kafka coordination |
| Redis | Job-status storage |

MySQL is configured as the persistent relational database used by the Job Service.

---

## Authentication

The application supports user registration and login using **JWT authentication**.

After successful login, the frontend stores the JWT and sends it with protected API requests.

```text
Register / Login
       ↓
Authentication Service
       ↓
JWT Token
       ↓
Authenticated API Requests
```

---

## Kafka-Based Processing

When a new job is submitted:

1. The Job Service saves the job.
2. A job event is published to Apache Kafka.
3. Worker Service consumes the event asynchronously.
4. The worker executes the job.
5. Execution failures can be retried.
6. The final status and result are sent back to the Job Service.
7. The updated job appears on the dashboard.

This decouples job submission from job execution and demonstrates an event-driven distributed architecture.

---

## Running Locally

### Prerequisites

Install:

- Java 17+
- Maven
- Node.js
- Docker
- Docker Compose
- MySQL

### Environment Variables

Create a local `.env` file with the required values.

Example variable names:

```env
MYSQL_PASSWORD=your_mysql_password
JWT_SECRET=your_jwt_secret
JWT_EXPIRATION=your_expiration_value
WORKER_API_KEY=your_worker_api_key
```

Do **not** commit real secrets to GitHub.

### Start the Platform

From the project root:

```bash
docker compose up --build
```

The Docker Compose configuration starts the distributed services required for local development.

---

## Production Deployment

The current public application uses a split cloud deployment.

```text
Internet
   │
   ▼
Vercel
   │
   │ React Frontend
   ▼
Railway
   │
   ├── Job Service
   ├── Worker Service
   ├── Kafka
   └── MySQL
```

The frontend is hosted on **Vercel**, while the distributed backend infrastructure runs on **Railway**.

---

## Example Processing Result

A successfully processed job follows a flow similar to:

```text
Job #17
LIVE-DEMO-TEST

Submitted
   ↓
Queued
   ↓
Processing
   ↓
Completed
```

This demonstrates end-to-end communication between the frontend, backend, Kafka infrastructure, worker service, and database.

---

## What This Project Demonstrates

This project demonstrates practical understanding of:

- Backend development with Spring Boot
- REST API design
- Event-driven architecture
- Apache Kafka messaging
- Producer-consumer patterns
- Asynchronous processing
- Microservice communication
- Authentication and authorization
- Relational database persistence
- Redis integration
- React frontend development
- Docker containerization
- Cloud deployment
- Failure handling and retry mechanisms

---

## Author

**Angoth Ramesh**

GitHub: https://github.com/AR021926