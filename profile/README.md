# 🐾 PawsStay Digital: Next-Gen Pet Boarding Ecosystem

Welcome to the **PawsStay Digital** engineering organization. We are building a cloud-native, microservices-based platform designed to streamline pet hotel operations and enhance the boarding experience for pet owners.

## 🏗 System Architecture (v2.0)

This project demonstrates a modern, event-driven microservices architecture optimized for scalability and reliability.

```mermaid
graph TD
    User((Pet Owner)) -->|HTTPS| Gateway[Spring Cloud Gateway]
    Admin((System Admin)) -->|HTTPS| Gateway

    subgraph "Service Layer"
        Gateway -->|Auth Check| Identity[Identity Service]
        Gateway -->|Manage| Pet[Pet Service]
        Gateway -->|Booking| Booking[Booking Service]
    end

    subgraph "Persistence & Cloud"
        Identity --> DB1[(PostgreSQL)]
        Pet --> DB2[(PostgreSQL)]
        Pet --> S3[AWS S3 - Pet Photos]
        Booking --> DB3[(PostgreSQL)]
    end

    subgraph "Messaging & Notifications"
        Booking -->|Async Event| SQS[AWS SQS / RabbitMQ]
        SQS --> Notification[Notification Service]
        Notification --> SES[AWS SES / SNS]
    end

    style Gateway fill:#f9f,stroke:#333,stroke-width:2px
    style SQS fill:#fff4dd,stroke:#d4a017,stroke-width:2px
```
