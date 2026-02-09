# 9. Architectural Design

## 9.1 Architecture Pattern: Three-Tier

The SMS uses a **three-tier (layered) architecture**, separating the system into Presentation, Business Logic, and Data tiers. This is the most appropriate pattern for a mid-sized web application because it provides clear separation of concerns, allows independent scaling of each tier, and is well-supported by the chosen technology stack.

```mermaid
graph TB
    subgraph Presentation["Presentation Tier (Client)"]
        Browser[Web Browser]
        React[React.js SPA]
    end

    subgraph Logic["Business Logic Tier (Server)"]
        API[REST API - Node.js / Express]
        Auth[Authentication Middleware - JWT]
        BL[Business Services]
        Val[Validation Layer]
    end

    subgraph Data["Data Tier"]
        MySQL[(MySQL Database)]
        FileStore[(File Storage - S3)]
        Cache[(Redis Cache)]
    end

    subgraph External["External Services"]
        SMTP[Email Service - SMTP]
    end

    Browser --> React
    React -->|HTTPS / REST| API
    API --> Auth
    Auth --> BL
    BL --> Val
    Val --> MySQL
    BL --> FileStore
    BL --> Cache
    BL --> SMTP

    style Presentation fill:#e3f2fd
    style Logic fill:#fff3e0
    style Data fill:#e8f5e9
    style External fill:#fce4ec
```

## 9.2 Technology Stack

| Layer | Technology | Justification |
|-------|-----------|---------------|
| Frontend | React.js | Component-based, large ecosystem, fast rendering |
| Backend | Node.js + Express | Full-stack JavaScript, async I/O, scalable |
| Database | MySQL | Relational data model, ACID compliance, mature tooling |
| Cache | Redis | Session storage, frequently accessed data (dashboards) |
| File Storage | AWS S3 | Scalable, reliable storage for assignment submissions |
| Authentication | JWT + bcrypt | Stateless auth, secure password hashing |
| API Style | RESTful | Industry standard, well-tooled, easy to test |
| Email | SMTP (SendGrid) | Reliable transactional email delivery |

## 9.3 Component Diagram

```mermaid
graph LR
    subgraph Frontend["Frontend (React)"]
        Login[Login Page]
        SD[Student Dashboard]
        TD[Teacher Dashboard]
        AD[Admin Dashboard]
        PD[Parent Dashboard]
    end

    subgraph Backend["Backend (Node.js)"]
        AuthC[Auth Controller]
        UserC[User Controller]
        CourseC[Course Controller]
        AttC[Attendance Controller]
        AssignC[Assignment Controller]
        GradeC[Grade Controller]
        ReportC[Report Controller]
    end

    subgraph Services["Business Services"]
        AuthS[Auth Service]
        EnrollS[Enrollment Service]
        AttS[Attendance Service]
        SubS[Submission Service]
        GradeS[Grade Calculation Service]
        NotifS[Notification Service]
    end

    subgraph Data["Data Layer"]
        UserR[User Repository]
        CourseR[Course Repository]
        AttR[Attendance Repository]
        AssignR[Assignment Repository]
        GradeR[Grade Repository]
    end

    Frontend -->|REST API| Backend
    Backend --> Services
    Services --> Data
    NotifS -->|SMTP| Email[Email Service]
    SubS -->|S3 API| Storage[File Storage]
    Data --> DB[(MySQL)]
```

## 9.4 Architecture Decisions

| Decision | Choice | Alternatives Considered | Rationale |
|----------|--------|------------------------|-----------|
| Architecture style | Three-tier monolith | Microservices | Simpler for a small team; microservices add overhead without clear benefit at this scale. |
| Frontend rendering | Single Page App (SPA) | Server-side rendering | Better user experience for dashboard-heavy app; API reuse for future mobile app. |
| Database | Relational (MySQL) | NoSQL (MongoDB) | Data is highly relational (students → enrollments → courses); ACID needed for grades. |
| Authentication | JWT tokens | Session cookies | Stateless auth scales better; works well with SPA architecture. |
| File storage | S3 (external) | Local file system | Scalable, durable, and separates file storage from application server. |

## 9.5 Deployment View

```mermaid
graph TB
    subgraph Client["Client Devices"]
        Desktop[Desktop Browser]
        Tablet[Tablet Browser]
        Mobile[Mobile Browser]
    end

    subgraph CDN["Content Delivery"]
        CF[CloudFront CDN]
    end

    subgraph Server["Application Server"]
        LB[Load Balancer]
        App1[App Instance 1]
        App2[App Instance 2]
    end

    subgraph DataStores["Data Stores"]
        DB[(MySQL Primary)]
        DBR[(MySQL Replica)]
        Redis[(Redis)]
        S3[(S3 Bucket)]
    end

    Client -->|HTTPS| CF
    CF -->|Static Assets| S3
    CF -->|API Requests| LB
    LB --> App1
    LB --> App2
    App1 --> DB
    App2 --> DB
    DB --> DBR
    App1 --> Redis
    App2 --> Redis
    App1 --> S3
    App2 --> S3
```

For version 1.0, a single application instance behind a load balancer is sufficient for 500 concurrent users. The architecture supports horizontal scaling (adding more app instances) to reach the 5,000-user target (NFR-015) without code changes.

---

[← Previous: Database Design](./08-database-design.md) | [Back to Index](./00-index.md) | [Next: Detailed Design →](./10-detailed-design.md)
