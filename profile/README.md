# 🐾 PawsStay Digital: Next-Gen Pet Boarding Ecosystem

Welcome to the **PawsStay Digital** engineering organization. We are building a cloud-native, microservices-based platform designed to streamline pet hotel operations and enhance the boarding experience for pet owners.

## 🏗 System Architecture (v2.1)

This project demonstrates a modern, event-driven microservices architecture optimized for scalability and reliability.

```mermaid
graph TD
    %% Layer 1: Frontend Application
    Client([React Frontend Application])

    %% Hidden layout to guide the top and outer left/right positioning
    subgraph Top_Layout [ ]
        direction LR
        Gateway_Layer ~~~ External_IdP
    end
    style Top_Layout fill:none,stroke:none

    %% External Identity Providers (Right Side)
    subgraph External_IdP [External Identity Providers OAuth2]
        Google[Google]
        Facebook[Facebook]
    end

    %% Gateway Layer (Top Center)
    subgraph Gateway_Layer [Gateway Layer]
        Gateway[API Gateway<br/>Spring Cloud]
    end

    %% Hidden layout to guide the middle left/right positioning
    subgraph Mid_Layout [ ]
        direction LR
        Async_Messaging ~~~ Microservices
    end
    style Mid_Layout fill:none,stroke:none

    %% Async Messaging & Notifications (Left Side)
    subgraph Async_Messaging [Async Messaging & Notifications AWS]
        SQS[[AWS SQS<br/>Message Queue]]
        SNS((AWS SNS / SES<br/>Email Dispatch))
    end

    %% Microservices Core Layer (Center)
    subgraph Microservices [Microservices Layer Spring Boot 3 + Java 25]
        Booking[Booking Service]
        Pet[Pet Service<br/>Pet Profile Management]
        Owner[Owner Service]
        Auth[Auth Service<br/>Auth & JWT Issuer]
        Notify[Notification Service]
    end

    %% Persistence Layer (Bottom)
    subgraph Persistence_Layer [Persistence Layer]
        direction LR
        DB_Booking[(PostgreSQL<br/>Booking)]
        S3[AWS S3<br/>Pet Photos Storage]
        DB_Pet[(PostgreSQL<br/>Pet DB)]
        DB_Owner[(PostgreSQL<br/>Owner DB)]
    end

    %% --- Connections & Flow ---

    %% Frontend Connections
    Client -.->|4. Request API with Internal JWT| Gateway
    Client -.-> External_IdP

    %% 3rd Party Validation & Auth
    External_IdP --> Auth
    Auth -->|3. Check / Auto-register Owner| Owner

    %% Gateway Routing to Microservices
    Gateway -->|Route to Services| Booking
    Gateway --> Pet
    Gateway --> Owner
    Gateway --> Auth

    %% Async Flow (Cross-Boundary)
    Booking -->|5. Booking Confirmed, Send Event| SQS
    SQS -->|6. Listen & Consume Message| Notify
    Notify -->|7. Trigger Email Dispatch| SNS

    %% Database Connections (Downward)
    Booking -->|Read / Write| DB_Booking
    Pet -->|Upload Image Files| S3
    Pet -->|Read / Write| DB_Pet
    Owner -->|Read / Write| DB_Owner

    %% --- Styling Definitions (Customized for Dark Mode from reference image) ---
    classDef dark fill:#111,stroke:#666,stroke-width:1px,color:#eee;
    classDef gateway fill:#FF00FF,stroke:#666,stroke-width:1px,color:#fff;
    classDef aws fill:#111,stroke:#FF9900,stroke-width:2px,color:#FF9900;
    classDef user fill:#666,stroke:#666,stroke-width:1px,color:#eee;
    classDef db fill:#111,stroke:#666,stroke-width:1px,color:#eee;

    %% --- Apply Classes ---
    class Client user;
    class Gateway gateway;
    class Auth,Owner,Pet,Booking,Notify dark;
    class SQS,SNS,S3 aws;
    class DB_Owner,DB_Pet,DB_Booking db;
    class Google,Facebook dark;

    %% --- Apply Subgraph Styles for Dark Mode ---
    style External_IdP fill:#222,stroke:#666,color:#eee;
    style Gateway_Layer fill:#222,stroke:#666,color:#eee;
    style Async_Messaging fill:#222,stroke:#666,color:#eee;
    style Microservices fill:#222,stroke:#666,color:#eee;
    style Persistence_Layer fill:#222,stroke:#666,color:#eee;
```
