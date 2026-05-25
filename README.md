# E-Commerce Microservices Platform

![Status](https://img.shields.io/badge/Status-Active-green?style=flat-square)
![Version](https://img.shields.io/badge/Version-1.2.0-blue?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

A scalable, distributed e-commerce platform built with modern microservices architecture. Demonstrates inter-service communication, API gateways, and containerization.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Services](#services)
- [Getting Started](#getting-started)
- [Deployment](#deployment)

## 🎯 Overview

**Status:** Production-Ready  
**Category:** Microservices Architecture  
**Purpose:** Scalable e-commerce backend platform

This project demonstrates enterprise-grade microservices patterns including service discovery, load balancing, API gateways, and event-driven architecture.

### Key Features

- ✅ Independent, scalable microservices
- ✅ API Gateway for request routing
- ✅ Service-to-service communication
- ✅ Event-driven architecture (message queues)
- ✅ Containerized with Docker
- ✅ Kubernetes-ready manifests
- ✅ Database per service pattern

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Load Balancer / DNS                   │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│              API Gateway (Kong/Express)                  │
│   - Request Routing                                      │
│   - Authentication                                       │
│   - Rate Limiting                                        │
│   - Request Logging                                      │
└──────┬──────┬──────┬──────┬──────┬──────┬──────┬────────┘
       │      │      │      │      │      │      │
   ┌───▼─┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐
   │User │ │Auth │ │Order│ │ Pay  │ │ Inv │ │Cart │ │Not  │
   │Serv │ │Serv │ │Serv │ │Serv │ │Serv │ │Serv │ │Serv │
   └─┬───┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘ └──┬──┘
     │       │      │      │      │      │      │
   ┌─▼───┐ ┌─▼────┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐ ┌──▼──┐
   │ DB1 │ │ DB2  │ │ DB3 │ │ DB4 │ │ DB5 │ │ DB6 │ │ DB7 │
   └─────┘ └──────┘ └─────┘ └─────┘ └─────┘ └─────┘ └─────┘

         ┌─────────────────────────────────┐
         │  Message Queue (RabbitMQ/Kafka) │
         │  - Event Streaming              │
         │  - Async Communication          │
         └─────────────────────────────────┘

         ┌─────────────────────────────────┐
         │   Service Discovery (Consul)     │
         │   - Service Registry            │
         │   - Health Checks               │
         └─────────────────────────────────┘
```

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| **Language** | Java / Node.js / Python |
| **Framework** | Spring Boot / Express |
| **API Gateway** | Spring Cloud Gateway |
| **Message Broker** | RabbitMQ / Apache Kafka |
| **Databases** | PostgreSQL, MongoDB, Redis |
| **Containerization** | Docker, Docker Compose |
| **Orchestration** | Kubernetes |
| **Service Discovery** | Eureka / Consul |
| **Monitoring** | Prometheus, Grafana, ELK |

## 🔧 Services

### 1. **User Service**
```
Port: 3001
Purpose: User authentication, registration, profile management
Database: PostgreSQL
```

### 2. **Auth Service**
```
Port: 3002
Purpose: JWT token generation, token validation
Database: Redis (token cache)
```

### 3. **Order Service**
```
Port: 3003
Purpose: Order creation, order management, status tracking
Database: MongoDB
```

### 4. **Payment Service**
```
Port: 3004
Purpose: Payment processing, transactions, refunds
Database: PostgreSQL
```

### 5. **Inventory Service**
```
Port: 3005
Purpose: Product catalog, stock management
Database: PostgreSQL
```

### 6. **Cart Service**
```
Port: 3006
Purpose: Shopping cart management
Database: Redis
```

### 7. **Notification Service**
```
Port: 3007
Purpose: Email, SMS notifications
Database: MongoDB (logs)
```

## 📁 Project Structure

```
ecomm-micro/
├── api-gateway/            # Main entry point
├── services/
│   ├── user-service/
│   │   ├── src/
│   │   ├── Dockerfile
│   │   └── pom.xml
│   ├── auth-service/
│   ├── order-service/
│   ├── payment-service/
│   ├── inventory-service/
│   ├── cart-service/
│   └── notification-service/
├── shared/                 # Shared libraries
│   ├── models/
│   ├── utils/
│   └── constants/
├── kubernetes/             # K8s manifests
│   ├── deployments/
│   ├── services/
│   └── configmaps/
├── docker-compose.yml      # Local development
├── docker-compose.prod.yml # Production
└── README.md
```

## 🚀 Getting Started

### Prerequisites

- Docker & Docker Compose
- Java 8+ (or language of choice)
- Git

### Installation - Docker (Recommended)

```bash
# Clone the repository
git clone https://github.com/divyaaojha/ecomm-micro.git
cd ecomm-micro

# Start all services
docker-compose up -d

# View logs
docker-compose logs -f
```

### Installation - Local Development

```bash
# Install dependencies for each service
for dir in services/*/; do
  cd "$dir" && mvn install && cd ../..
done

# Set environment variables
cp .env.example .env

# Start services (in separate terminals)
cd services/user-service && mvn spring-boot:run
cd services/order-service && mvn spring-boot:run
# ... start other services
```

## 🔌 API Endpoints

### User Service
```
POST   /api/users/register
POST   /api/users/login
GET    /api/users/:id
PUT    /api/users/:id
DELETE /api/users/:id
```

### Order Service
```
POST   /api/orders
GET    /api/orders/:id
GET    /api/orders/user/:userId
PUT    /api/orders/:id
DELETE /api/orders/:id
```

### Payment Service
```
POST   /api/payments
GET    /api/payments/:id
POST   /api/payments/:id/refund
```

## 🌐 Service Communication

### Synchronous (REST)
```bash
# User Service calls Auth Service
curl http://auth-service:3002/verify
```

### Asynchronous (Message Queue)
```
Event: order.created → Notification Service → Send email
Event: payment.completed → Inventory Service → Update stock
```

## 🐳 Docker Deployment

```bash
# Build all services
docker-compose build

# Push to registry
docker tag ecomm-user:latest myregistry/ecomm-user:v1.0
docker push myregistry/ecomm-user:v1.0

# Deploy
docker-compose -f docker-compose.prod.yml up -d
```

## ☸️ Kubernetes Deployment

```bash
# Create namespace
kubectl create namespace ecomm

# Deploy services
kubectl apply -f kubernetes/deployments/ -n ecomm
kubectl apply -f kubernetes/services/ -n ecomm

# Verify deployment
kubectl get pods -n ecomm
kubectl get svc -n ecomm
```

## 📊 Monitoring & Logging

### Prometheus Metrics
```
http://localhost:9090/metrics
```

### Grafana Dashboards
```
http://localhost:3000
Username: admin
Password: admin
```

### ELK Stack (Elasticsearch, Logstash, Kibana)
```
http://localhost:5601
```

## 🔒 Security

- ✅ JWT-based authentication
- ✅ Rate limiting per service
- ✅ CORS configuration
- ✅ Data encryption in transit (TLS)
- ✅ Input validation & sanitization
- ✅ API key management

## 🤝 Contributing

1. Create a feature branch
2. Make changes to a specific service
3. Run tests: `mvn test`
4. Submit a Pull Request

## 📚 Documentation

- [API Documentation](./API_DOCS.md)
- [Deployment Guide](./DEPLOYMENT.md)
- [Architecture Decision Records](./ADR.md)
- [Database Schema](./DATABASE.md)

## 📄 License

MIT License - see [LICENSE](LICENSE)

## 👤 Author

**Divya Ojha** - [GitHub](https://github.com/divyaaojha)

---

**Last Updated:** May 25, 2026  
**Status:** Production Ready ✅
