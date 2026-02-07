# 🕵️ Mission Impossible - Spy Mission Management System

A cloud-native, full-stack web application for managing spy missions. Built with Spring Boot and deployed on AWS EKS with a complete CI/CD pipeline, security scanning, and observability stack.

![Java](https://img.shields.io/badge/Java-11-orange?style=flat-square&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-2.6.0-brightgreen?style=flat-square&logo=spring)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue?style=flat-square&logo=docker)
![Kubernetes](https://img.shields.io/badge/Kubernetes-EKS-326CE5?style=flat-square&logo=kubernetes)
![Jenkins](https://img.shields.io/badge/CI%2FCD-Jenkins-red?style=flat-square&logo=jenkins)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Application Features](#application-features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [CI/CD Pipeline](#cicd-pipeline)
- [Kubernetes Deployment](#kubernetes-deployment)
- [Monitoring & Observability](#monitoring--observability)
- [Getting Started](#getting-started)
- [API Endpoints](#api-endpoints)
- [Project Structure](#project-structure)

---

## Overview

Mission Impossible is a mission management system where users can create, view, edit, and delete spy missions for various agents. Each mission includes details like agent name, mission title, and assigned gadgets.

The application demonstrates a complete DevOps lifecycle from development to production, including containerization, CI/CD automation, security scanning, cloud-native deployment, and comprehensive monitoring.

---

## Application Features

- **CRUD Operations** - Create, Read, Update, and Delete missions
- **Agent-Based Views** - View missions filtered by agent
- **Gadget Assignment** - Assign up to 2 gadgets per mission
- **Session Management** - Persistent user sessions for seamless navigation
- **Responsive UI** - Thymeleaf-based dynamic HTML templates
- **In-Memory Database** - H2 database with auto-initialization

---

## Tech Stack

### Application Layer
| Technology | Purpose |
|------------|---------|
| Java 11 | Core programming language |
| Spring Boot 2.6.0 | Application framework |
| Spring Data JDBC | Database connectivity |
| Thymeleaf | Server-side templating |
| Lombok | Boilerplate reduction |
| H2 Database | In-memory relational database |
| Maven | Build & dependency management |

### DevOps & Infrastructure
| Technology | Purpose |
|------------|---------|
| Docker | Containerization |
| Docker Hub | Container registry |
| Jenkins | CI/CD automation |
| SonarQube | Static code analysis & quality gates |
| Trivy | Vulnerability scanning (source & images) |
| JaCoCo | Code coverage reporting |

### Cloud & Orchestration
| Technology | Purpose |
|------------|---------|
| AWS EKS | Managed Kubernetes service |
| Kubernetes | Container orchestration |
| LoadBalancer | Traffic exposure |
| Horizontal Pod Autoscaler | Dynamic scaling |

### Observability
| Technology | Purpose |
|------------|---------|
| Prometheus | Metrics collection & scraping |
| Grafana | Dashboards & visualization |

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CI/CD Pipeline                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐  │
│  │  GitHub  │──▶│ Jenkins  │──▶│SonarQube │──▶│  Trivy   │──▶│Docker Hub│  │
│  │   Repo   │   │  Build   │   │ Analysis │   │   Scan   │   │  Push    │  │
│  └──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘  │
└───────────────────────────────────────┬─────────────────────────────────────┘
                                        │
                                        ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              AWS EKS Cluster                                 │
│  ┌────────────────────────────────────────────────────────────────────────┐ │
│  │                         Kubernetes Namespace                            │ │
│  │  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐             │ │
│  │  │   Pod (1)    │    │   Pod (2)    │    │   Pod (n)    │             │ │
│  │  │ ┌──────────┐ │    │ ┌──────────┐ │    │ ┌──────────┐ │             │ │
│  │  │ │ Mission  │ │    │ │ Mission  │ │    │ │ Mission  │ │             │ │
│  │  │ │   App    │ │    │ │   App    │ │    │ │   App    │ │  ◀── HPA   │ │
│  │  │ └──────────┘ │    │ └──────────┘ │    │ └──────────┘ │             │ │
│  │  └──────────────┘    └──────────────┘    └──────────────┘             │ │
│  │         │                   │                   │                      │ │
│  │         └───────────────────┼───────────────────┘                      │ │
│  │                             │                                          │ │
│  │                    ┌────────▼────────┐                                │ │
│  │                    │     Service     │                                │ │
│  │                    │  (LoadBalancer) │                                │ │
│  │                    └────────┬────────┘                                │ │
│  └─────────────────────────────┼──────────────────────────────────────────┘ │
└─────────────────────────────────┼───────────────────────────────────────────┘
                                  │
                                  ▼
                           ┌─────────────┐
                           │    Users    │
                           └─────────────┘

┌─────────────────────────────────────────────────────────────────────────────┐
│                           Observability Stack                                │
│  ┌────────────────────────────┐    ┌────────────────────────────┐          │
│  │        Prometheus          │───▶│         Grafana            │          │
│  │   (Metrics Collection)     │    │   (Dashboards & Alerts)    │          │
│  └────────────────────────────┘    └────────────────────────────┘          │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## CI/CD Pipeline

The Jenkins pipeline automates the entire software delivery process:

### Pipeline Stages

```
┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐
│ Checkout│──▶│  Build  │──▶│  Test   │──▶│SonarQube│──▶│  Trivy  │──▶│ Docker  │
│         │   │ (Maven) │   │  & JaCoCo│  │ Analysis│   │  Scan   │   │  Build  │
└─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘   └─────────┘
                                                                            │
┌─────────────────────────────────────────────────────────────────────────────┘
│
▼
┌─────────┐   ┌─────────┐   ┌─────────┐
│  Push   │──▶│  Deploy │──▶│ Verify  │
│ to Hub  │   │ to EKS  │   │         │
└─────────┘   └─────────┘   └─────────┘
```

### Key Features

| Stage | Description |
|-------|-------------|
| **Build** | Maven build with dependency resolution |
| **Test** | Unit tests with JaCoCo coverage |
| **SonarQube** | Static analysis, code smells, security hotspots, quality gates |
| **Trivy Scan** | Vulnerability scanning of source code and container images |
| **Docker Build** | Multi-stage Docker image creation |
| **Push to Hub** | Secure image delivery to Docker Hub |
| **Deploy to EKS** | Rolling deployment to Kubernetes |

---

## Kubernetes Deployment

### Deployment Configuration

```yaml
# Replicas: 2 (High Availability)
# Image: adijaiswal/mission:latest
# Port: 8080
# Resource Management: HPA enabled
```

### Features

- **Deployment** - Manages pod replicas with rolling updates
- **Service (LoadBalancer)** - Exposes application externally via AWS ELB
- **Horizontal Pod Autoscaler** - Scales pods based on CPU/memory utilization
- **High Availability** - Multiple replicas across availability zones

### Kubernetes Resources

```bash
# Deployment
kubectl get deployment mission-deployment

# Service
kubectl get svc mission-ssvc

# Pods
kubectl get pods -l app=mission

# HPA
kubectl get hpa
```

---

## Monitoring & Observability

### Prometheus
- Scrapes application and infrastructure metrics
- Service discovery for Kubernetes pods
- Alert rules for critical conditions

### Grafana Dashboards
- **Application Health** - Request rates, latency, error rates
- **JVM Metrics** - Heap usage, GC stats, thread counts
- **Kubernetes** - Pod status, resource utilization, node health
- **Infrastructure** - CPU, memory, network, disk I/O

---

## Getting Started

### Prerequisites

- Java 11+
- Maven 3.6+
- Docker
- kubectl (for deployment)

### Local Development

```bash
# Clone the repository
git clone https://github.com/yourusername/Mission-Impossible.git
cd Mission-Impossible

# Build the application
./mvnw clean package

# Run locally
./mvnw spring-boot:run

# Access the application
open http://localhost:8080
```

### Docker Build & Run

```bash
# Build Docker image
docker build -t mission-impossible:latest .

# Run container
docker run -p 8080:8080 mission-impossible:latest
```

### Deploy to Kubernetes

```bash
# Apply deployment and service
kubectl apply -f ds.yml

# Verify deployment
kubectl get pods -l app=mission
kubectl get svc mission-ssvc

# Get external IP
kubectl get svc mission-ssvc -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Home page |
| GET | `/viewMissions?agent={name}` | View missions by agent |
| GET | `/addMission` | Create mission form |
| POST | `/createMission` | Submit new mission |
| GET | `/editMission/{id}` | Edit mission form |
| POST | `/updateMission` | Update existing mission |
| GET | `/deleteMission/{id}` | Delete a mission |

---

## Project Structure

```
Mission-Impossible/
├── src/
│   ├── main/
│   │   ├── java/ca/sheridancollege/
│   │   │   ├── beans/
│   │   │   │   └── Mission.java          # Domain model
│   │   │   ├── controllers/
│   │   │   │   └── HomeController.java   # MVC controller
│   │   │   ├── database/
│   │   │   │   └── DatabaseAccess.java   # JDBC operations
│   │   │   └── Assignment3MihyeBangApplication.java
│   │   └── resources/
│   │       ├── templates/                 # Thymeleaf templates
│   │       │   ├── index.html
│   │       │   ├── create_mission.html
│   │       │   ├── edit_mission.html
│   │       │   └── view_mission.html
│   │       ├── schema.sql                 # Database schema
│   │       ├── data.sql                   # Initial data
│   │       └── application.properties
│   └── test/                              # Unit tests
├── Dockerfile                             # Container definition
├── ds.yml                                 # K8s Deployment & Service
├── pom.xml                                # Maven configuration
└── README.md
```

---

## Database Schema

```sql
CREATE TABLE missions (
    id LONG PRIMARY KEY AUTO_INCREMENT,
    agent VARCHAR(50),
    title VARCHAR(50),
    gadget1 VARCHAR(50),
    gadget2 VARCHAR(50)
);
```

### Sample Data

| Agent | Mission | Gadgets |
|-------|---------|---------|
| Johnny English | Rescue the Queen | Exploding Cigar, Voice Controlled Rolls Royce |
| Natasha Romanova | Kill Iron Man | Armored Suit, Indestructible Pole |

---

## License

This project is for educational and demonstration purposes.

---

## Author

Built with ☕ and 🔧
