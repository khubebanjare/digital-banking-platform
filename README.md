# Digital Payment Platform 💳

A high-performance, secure, resilient, and cloud-native Digital Payment Platform built using Java, Spring Boot, Microservices, Apache Kafka, Docker, and Kubernetes.

This platform simulates real-world fintech infrastructure used by modern digital banking and payment companies for:
- wallet management
- peer-to-peer transfers
- merchant payouts
- payment orchestration
- distributed transaction management
- fraud detection
- ledger consistency
- event-driven processing

---

# 🚀 Project Goals

The objective of this project is to design and build a production-style fintech ecosystem demonstrating:

- Enterprise Microservices Architecture
- Distributed Systems Design
- Event-Driven Architecture
- Kafka-based Async Processing
- Secure Payment Processing
- Wallet & Ledger Management
- Cloud-Native Deployment
- High Scalability & Fault Tolerance
- Kubernetes Orchestration
- Production Observability

---

# 🏗️ Enterprise Architecture

```text
                                       +----------------------+
                                       |  Client Applications |
                                       |   Web / Mobile App   |
                                       +----------------------+
                                                   |
                                                   ▼
                                    +--------------------------------+
                                    |      API Gateway Service       |
                                    | Spring Cloud Gateway + JWT     |
                                    +--------------------------------+
                                                   |
        --------------------------------------------------------------------------------------------------
        |                     |                    |                    |                  |               |
        ▼                     ▼                    ▼                    ▼                  ▼               ▼
+----------------+  +----------------+  +-------------------+  +----------------+  +--------------+  +----------------+
| Auth & Identity|  | Wallet Service |  | Payment Gateway   |  | Payment Process|  | Ledger Svc  |  | TransactionSvc |
|    Service     |  |                |  |    Integrator     |  |    Service     |  |              |  |                |
+----------------+  +----------------+  +-------------------+  +----------------+  +--------------+  +----------------+
        |                     |                    |                    |                  |               |
        ---------------------------------------------------------------------------------------------------
                                                   |
                                                   ▼
                                          +------------------+
                                          |    Kafka Bus     |
                                          +------------------+
                                                   |
                           ----------------------------------------------------------------
                           |                         |                          |
                           ▼                         ▼                          ▼
               +-------------------+   +------------------------+   +----------------------+
               | Notification Svc  |   | Fraud Detection Svc   |   | Analytics Service    |
               +-------------------+   +------------------------+   +----------------------+
                                                   |
                                                   ▼
                                   +--------------------------------+
                                   | PostgreSQL / Redis / MongoDB  |
                                   +--------------------------------+
```

---

# 🧩 Core Microservices

---

# 1. Auth & Identity Service

## Responsibilities
- User Registration
- Authentication
- JWT Token Generation
- RBAC Authorization
- Session Management

## Technology
- Spring Security
- JWT (RSA-256)
- PostgreSQL

## Port
```text
8081
```

---

# 2. Wallet Service

## Responsibilities
- Wallet Creation
- Balance Management
- Balance Reservation
- Distributed Locking
- Wallet Validation

## Features
- Redis Distributed Locking
- Atomic Balance Updates
- Low Latency Access

## Port
```text
8082
```

---

# 3. Payment Gateway Integrator

## Responsibilities
- External Bank Integration
- UPI Integration
- Card Gateway Simulation
- Payment Provider Routing
- External API Retry

## Features
- Retry Mechanism
- Circuit Breaker
- Provider Failover

## Port
```text
8083
```

---

# 4. Payment Processing Service

## Responsibilities
- Payment Orchestration
- Debit/Credit Execution
- Idempotency Handling
- Payment Lifecycle Management
- Saga Coordination
- Kafka Event Publishing

## Payment Flow
```text
PENDING
   ↓
PROCESSING
   ↓
SUCCESS / FAILED
```

## Port
```text
8084
```

---

# 5. Ledger Service

## Responsibilities
- Immutable Ledger Entries
- Financial Audit Trails
- Double Entry Accounting
- Ledger Reconciliation

## Storage
- Apache Hudi
- PostgreSQL

## Port
```text
8085
```

---

# 6. Transaction Service

## Responsibilities
- Transaction History
- Reporting
- Transaction Search
- Transaction Metadata

## Port
```text
8086
```

---

# 7. Notification Service

## Responsibilities
- Email Notifications
- SMS Notifications
- Push Notifications
- Kafka Consumer Processing

## Port
```text
8087
```

---

# 8. Fraud Detection Service

## Responsibilities
- Fraud Analysis
- Velocity Checks
- Risk Scoring
- Suspicious Activity Detection

## Port
```text
8088
```

---

# 9. Analytics Service

## Responsibilities
- Payment Analytics
- Real-Time Metrics
- Kafka Stream Aggregation
- Dashboard Data

## Port
```text
8089
```

---

# 🔄 Event-Driven Architecture

Apache Kafka is used for asynchronous communication between services.

## Benefits
- Loose Coupling
- Scalability
- Fault Tolerance
- Retry Handling
- Event Replay

---

# 📌 Kafka Topics

| Topic | Producer | Consumer |
|---|---|---|
| user-created | auth-service | notification-service |
| payment-created | payment-processing-service | ledger-service |
| payment-completed | payment-processing-service | notification-service |
| fraud-check | payment-processing-service | fraud-detection-service |
| ledger-entry-created | ledger-service | analytics-service |
| transaction-created | transaction-service | analytics-service |

---

# 🔐 Security Architecture

## Authentication
- JWT Authentication
- RSA-256 Asymmetric Encryption
- Stateless Authentication

## Authorization
- Role-Based Access Control (RBAC)
- Scope-Based Authorization

## Data Protection
- AES-256 Encryption
- Secure Secret Management
- Kubernetes Network Policies

---

# 📡 API Documentation

## Register User

```http
POST /api/v1/auth/register
```

### Request
```json
{
  "username": "khubebanjare",
  "password": "SecurePassword123!",
  "email": "khube.banjare@example.com",
  "role": "USER"
}
```

### Response
```json
{
  "userId": "usr_99834211a",
  "status": "SUCCESS",
  "createdAt": "2026-05-30T14:00:00Z"
}
```

---

## Login

```http
POST /api/v1/auth/login
```

### Request
```json
{
  "username": "khubebanjare",
  "password": "SecurePassword123!"
}
```

### Response
```json
{
  "token_type": "Bearer",
  "access_token": "jwt-token",
  "expires_in": 3600,
  "refresh_token": "refresh-token"
}
```

---

## Wallet Details

```http
GET /api/v1/wallet/{walletId}
```

### Response
```json
{
  "walletId": "wlt_55312992b",
  "currency": "INR",
  "availableBalance": 45250.75,
  "reservedBalance": 0.00,
  "updatedAt": "2026-05-30T19:15:30Z"
}
```

---

## Wallet Transfer

```http
POST /api/v1/wallet/transfer
```

### Request
```json
{
  "destinationWalletId": "wlt_99482110c",
  "amount": 1500.00,
  "currency": "INR",
  "description": "Dinner split reimbursement"
}
```

### Response
```json
{
  "transactionReference": "txn_992821aa",
  "status": "PROCESSING"
}
```

---

# 🐳 Docker Setup

## Prerequisites
- Docker Engine >= 24
- Docker Compose V2

---

## Build Services

```bash
./mvnw clean package -DskipTests

docker build -t digital-payment/auth-service:1.0.0 ./auth-service
docker build -t digital-payment/wallet-service:1.0.0 ./wallet-service
```

---

## Docker Compose

```yaml
version: '3.8'

services:

  postgres-db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: payment_auth
      POSTGRES_USER: dev_admin
      POSTGRES_PASSWORD: SecretPassword
    ports:
      - "5432:5432"

  redis-cache:
    image: redis:7-alpine
    ports:
      - "6379:6379"

  kafka-broker:
    image: confluentinc/cp-kafka:7.3.0
    ports:
      - "9092:9092"

  auth-service:
    image: digital-payment/auth-service:1.0.0
    build: ./auth-service
    ports:
      - "8081:8081"

  wallet-service:
    image: digital-payment/wallet-service:1.0.0
    build: ./wallet-service
    ports:
      - "8082:8082"
```

---

## Start Infrastructure

```bash
docker compose up -d
```

---

# ☸️ Kubernetes Deployment

## Features
- Deployments
- Services
- ConfigMaps
- Secrets
- Resource Limits
- Readiness/Liveness Probes
- HPA Autoscaling

---

## Example Deployment

```yaml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: wallet-service-deployment
  namespace: payment-prod

spec:
  replicas: 3

  selector:
    matchLabels:
      app: wallet-service

  template:
    metadata:
      labels:
        app: wallet-service

    spec:
      containers:
      - name: wallet-service-container
        image: digital-payment/wallet-service:1.0.0

        ports:
        - containerPort: 8082

        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8082

        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8082
```

---

# 📊 Observability Stack

## Monitoring
- Prometheus
- Grafana
- JVM Metrics
- Kafka Metrics

## Distributed Tracing
- OpenTelemetry
- Zipkin

## Logging
- ELK Stack
- Correlation IDs

---

# ⚡ Advanced Enterprise Features

- Saga Pattern
- Distributed Transactions
- Resilience4j
- Circuit Breakers
- Retry Policies
- Dead Letter Queue
- Distributed Locking
- Event Replay
- CQRS
- Event Sourcing

---

# 📁 Project Structure

```text
digital-payment-platform/
│
├── auth-service/
├── wallet-service/
├── payment-gateway-service/
├── payment-processing-service/
├── ledger-service/
├── transaction-service/
├── notification-service/
├── fraud-detection-service/
├── analytics-service/
│
├── api-gateway/
├── discovery-server/
├── config-server/
│
├── docker/
├── kubernetes/
├── docs/
├── architecture/
│
├── docker-compose.yml
└── README.md
```

---

# 🧠 Enterprise Concepts Demonstrated

- Distributed Systems
- Event-Driven Architecture
- Payment Processing Systems
- Ledger Management
- Kafka Messaging
- Docker Containerization
- Kubernetes Orchestration
- Distributed Locking
- JWT Security
- API Gateway Pattern
- Service Discovery
- Fault Tolerance
- Cloud-Native Engineering

---

# 🎯 Interview Value

This project helps explain:
- Microservices Architecture
- Kafka Internals
- Distributed Transactions
- Payment Processing
- Docker & Kubernetes
- Event-Driven Systems
- Scalability
- System Design
- Cloud-Native Deployment
- Enterprise Backend Engineering

---

# 🛣️ Development Roadmap

```text
Monolith
   ↓
Microservices
   ↓
Kafka Integration
   ↓
Docker
   ↓
Kubernetes
   ↓
Production Engineering
```

---

# 📄 License

MIT License
