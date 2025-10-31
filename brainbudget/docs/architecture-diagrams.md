# System Architecture Diagrams

## 1. High-Level System Architecture

```mermaid
graph TB
    subgraph "Client Layer"
        WEB[Web App<br/>Next.js]
        MOBILE[Mobile App<br/>React Native]
        PWA[PWA]
    end

    subgraph "CDN & Edge"
        CDN[CDN<br/>Cloudflare/Vercel]
        EDGE[Edge Functions]
    end

    subgraph "API Gateway Layer"
        GATEWAY[API Gateway<br/>Kong/Nginx]
        LB[Load Balancer]
    end

    subgraph "Authentication Layer"
        AUTH[Auth Service]
        OAUTH[OAuth Provider]
    end

    subgraph "Core Services"
        USER[User Service]
        EXPENSE[Expense Service]
        CATEGORY[Category Service]
        BUDGET[Budget Service]
        ANALYTICS[Analytics Service]
    end

    subgraph "AI/ML Services"
        OCR[OCR Service]
        AI[AI Service<br/>Categorization/Insights]
        ML[ML Models]
    end

    subgraph "Integration Services"
        STORAGE[Storage Service]
        NOTIFICATION[Notification Service]
        REPORT[Report Service]
        INTEGRATION[Integration Service<br/>Banks/APIs]
    end

    subgraph "Data Layer"
        PG[(PostgreSQL<br/>Primary DB)]
        REDIS[(Redis<br/>Cache)]
        ES[(ElasticSearch<br/>Search)]
        S3[(S3/R2<br/>File Storage)]
        QUEUE[Message Queue<br/>RabbitMQ/Redis]
    end

    subgraph "External Services"
        BANK[Banking APIs<br/>Plaid]
        PAYMENT[Payment APIs<br/>Stripe]
        EMAIL[Email Service<br/>Resend]
        SMS[SMS Service<br/>Twilio]
        VISION[Cloud Vision API]
        OPENAI[OpenAI API]
    end

    subgraph "Monitoring & Observability"
        SENTRY[Error Tracking<br/>Sentry]
        METRICS[Metrics<br/>Prometheus]
        LOGS[Logging<br/>Loki]
        APM[APM<br/>DataDog]
    end

    WEB --> CDN
    MOBILE --> CDN
    PWA --> CDN
    CDN --> GATEWAY
    EDGE --> GATEWAY
    GATEWAY --> LB
    
    LB --> AUTH
    LB --> USER
    LB --> EXPENSE
    LB --> CATEGORY
    LB --> BUDGET
    LB --> ANALYTICS
    LB --> OCR
    LB --> AI
    LB --> STORAGE
    LB --> NOTIFICATION
    LB --> REPORT
    LB --> INTEGRATION

    AUTH -.-> OAUTH
    AUTH --> REDIS
    
    USER --> PG
    USER --> REDIS
    
    EXPENSE --> PG
    EXPENSE --> REDIS
    EXPENSE --> ES
    EXPENSE --> QUEUE
    
    CATEGORY --> PG
    CATEGORY --> REDIS
    
    BUDGET --> PG
    BUDGET --> REDIS
    BUDGET --> QUEUE
    
    ANALYTICS --> PG
    ANALYTICS --> REDIS
    ANALYTICS --> ES
    
    OCR --> S3
    OCR -.-> VISION
    OCR --> QUEUE
    
    AI --> PG
    AI --> ML
    AI -.-> OPENAI
    
    STORAGE --> S3
    
    NOTIFICATION --> QUEUE
    NOTIFICATION -.-> EMAIL
    NOTIFICATION -.-> SMS
    
    REPORT --> PG
    REPORT --> S3
    REPORT --> QUEUE
    
    INTEGRATION -.-> BANK
    INTEGRATION -.-> PAYMENT
    INTEGRATION --> PG
    
    AUTH -.-> SENTRY
    USER -.-> SENTRY
    EXPENSE -.-> SENTRY
    
    AUTH -.-> METRICS
    USER -.-> METRICS
    EXPENSE -.-> METRICS
    
    AUTH -.-> LOGS
    USER -.-> LOGS
    EXPENSE -.-> LOGS

    style WEB fill:#61dafb
    style MOBILE fill:#61dafb
    style PWA fill:#61dafb
    style GATEWAY fill:#ff6b6b
    style AUTH fill:#4ecdc4
    style PG fill:#336791
    style REDIS fill:#dc382d
    style S3 fill:#ff9900
    style QUEUE fill:#ff6600
```

## 2. Microservices Architecture Detail

```mermaid
graph LR
    subgraph "Frontend Applications"
        WEBAPP[Web Application]
        MOBILEAPP[Mobile Application]
    end

    subgraph "API Gateway"
        KONG[Kong Gateway<br/>- Routing<br/>- Rate Limiting<br/>- Auth Validation<br/>- Request Logging]
    end

    subgraph "Service Mesh"
        AUTH_SVC[🔐 Auth Service<br/>Port: 3001<br/>- JWT Management<br/>- OAuth2<br/>- 2FA]
        
        USER_SVC[👤 User Service<br/>Port: 3002<br/>- Profile CRUD<br/>- Preferences<br/>- Avatar Management]
        
        EXPENSE_SVC[💰 Expense Service<br/>Port: 3003<br/>- Expense CRUD<br/>- Search & Filter<br/>- Audit Trail]
        
        CATEGORY_SVC[📁 Category Service<br/>Port: 3004<br/>- Category Management<br/>- Budgets<br/>- Icons/Colors]
        
        STORAGE_SVC[📦 Storage Service<br/>Port: 3005<br/>- File Upload<br/>- Image Processing<br/>- Signed URLs]
        
        ANALYTICS_SVC[📊 Analytics Service<br/>Port: 3006<br/>- Aggregations<br/>- Trends<br/>- Insights]
        
        REPORT_SVC[📄 Report Service<br/>Port: 3007<br/>- PDF Generation<br/>- CSV Export<br/>- Scheduled Reports]
        
        OCR_SVC[🔍 OCR Service<br/>Port: 3008<br/>- Image Processing<br/>- Text Extraction<br/>- Data Parsing]
        
        AI_SVC[🤖 AI Service<br/>Port: 3009<br/>- Categorization<br/>- Anomaly Detection<br/>- Chatbot]
        
        BUDGET_SVC[💵 Budget Service<br/>Port: 3010<br/>- Budget CRUD<br/>- Alerts<br/>- Tracking]
        
        NOTIF_SVC[🔔 Notification Service<br/>Port: 3011<br/>- Email<br/>- Push<br/>- SMS]
        
        INTEGRATION_SVC[🔗 Integration Service<br/>Port: 3012<br/>- Bank Sync<br/>- Payment APIs<br/>- Webhooks]
    end

    subgraph "Event Bus"
        KAFKA[Apache Kafka / RabbitMQ<br/>- Event Publishing<br/>- Event Consumption<br/>- Dead Letter Queue]
    end

    subgraph "Data Stores"
        PG_AUTH[(Auth DB)]
        PG_USER[(User DB)]
        PG_EXPENSE[(Expense DB)]
        PG_ANALYTICS[(Analytics DB)]
        REDIS_CACHE[(Redis Cache)]
        S3_STORAGE[(S3 Storage)]
        ELASTICSEARCH[(Elasticsearch)]
    end

    WEBAPP --> KONG
    MOBILEAPP --> KONG
    
    KONG --> AUTH_SVC
    KONG --> USER_SVC
    KONG --> EXPENSE_SVC
    KONG --> CATEGORY_SVC
    KONG --> STORAGE_SVC
    KONG --> ANALYTICS_SVC
    KONG --> REPORT_SVC
    KONG --> OCR_SVC
    KONG --> AI_SVC
    KONG --> BUDGET_SVC
    KONG --> NOTIF_SVC
    KONG --> INTEGRATION_SVC
    
    AUTH_SVC --> PG_AUTH
    AUTH_SVC --> REDIS_CACHE
    
    USER_SVC --> PG_USER
    USER_SVC --> REDIS_CACHE
    USER_SVC --> KAFKA
    
    EXPENSE_SVC --> PG_EXPENSE
    EXPENSE_SVC --> REDIS_CACHE
    EXPENSE_SVC --> ELASTICSEARCH
    EXPENSE_SVC --> KAFKA
    
    CATEGORY_SVC --> PG_EXPENSE
    CATEGORY_SVC --> REDIS_CACHE
    
    STORAGE_SVC --> S3_STORAGE
    STORAGE_SVC --> KAFKA
    
    ANALYTICS_SVC --> PG_ANALYTICS
    ANALYTICS_SVC --> REDIS_CACHE
    
    REPORT_SVC --> PG_EXPENSE
    REPORT_SVC --> S3_STORAGE
    REPORT_SVC --> KAFKA
    
    OCR_SVC --> S3_STORAGE
    OCR_SVC --> KAFKA
    
    AI_SVC --> PG_EXPENSE
    AI_SVC --> KAFKA
    
    BUDGET_SVC --> PG_EXPENSE
    BUDGET_SVC --> REDIS_CACHE
    BUDGET_SVC --> KAFKA
    
    NOTIF_SVC --> KAFKA
    
    INTEGRATION_SVC --> PG_EXPENSE
    INTEGRATION_SVC --> KAFKA
```

## 3. Service Communication Patterns

```mermaid
sequenceDiagram
    participant Client
    participant Gateway
    participant Auth
    participant Expense
    participant Storage
    participant OCR
    participant AI
    participant Queue
    participant Notification

    Client->>Gateway: POST /api/expenses (with receipt)
    Gateway->>Auth: Validate JWT
    Auth-->>Gateway: Token Valid
    
    Gateway->>Storage: Upload Receipt
    Storage-->>Gateway: File URL
    
    Gateway->>Expense: Create Expense (with receipt URL)
    Expense->>Queue: Publish ExpenseCreated Event
    Expense-->>Gateway: Expense Created (201)
    Gateway-->>Client: Response
    
    Queue->>OCR: Consume ExpenseCreated Event
    OCR->>Storage: Download Receipt
    Storage-->>OCR: Image Data
    OCR->>OCR: Extract Text
    OCR->>Queue: Publish ReceiptProcessed Event
    
    Queue->>AI: Consume ReceiptProcessed Event
    AI->>AI: Categorize Expense
    AI->>Expense: Update Category & Merchant
    AI->>Queue: Publish ExpenseCategorized Event
    
    Queue->>Notification: Consume ExpenseCategorized Event
    Notification->>Client: Push Notification
```

## 4. Data Flow Architecture

```mermaid
graph TD
    subgraph "Ingestion Layer"
        UI[User Interface]
        API_UPLOAD[File Upload API]
        BANK_SYNC[Bank Sync Job]
        WEBHOOK[External Webhooks]
    end

    subgraph "Processing Layer"
        VALIDATION[Data Validation]
        TRANSFORMATION[Data Transformation]
        ENRICHMENT[Data Enrichment<br/>- Currency Conversion<br/>- Merchant Lookup<br/>- Category Suggestion]
    end

    subgraph "Storage Layer"
        HOT_STORAGE[(Hot Storage<br/>Last 90 days<br/>PostgreSQL)]
        WARM_STORAGE[(Warm Storage<br/>90 days - 1 year<br/>PostgreSQL Partitions)]
        COLD_STORAGE[(Cold Storage<br/>1+ years<br/>S3/Glacier)]
        CACHE[(Cache Layer<br/>Redis)]
    end

    subgraph "Analytics Pipeline"
        ETL[ETL Jobs]
        AGGREGATION[Data Aggregation]
        OLAP[(OLAP Database<br/>or Data Warehouse)]
    end

    subgraph "Serving Layer"
        API[REST/GraphQL API]
        SEARCH[Search API<br/>Elasticsearch]
        REAL_TIME[Real-time Updates<br/>WebSocket]
    end

    UI --> VALIDATION
    API_UPLOAD --> VALIDATION
    BANK_SYNC --> VALIDATION
    WEBHOOK --> VALIDATION
    
    VALIDATION --> TRANSFORMATION
    TRANSFORMATION --> ENRICHMENT
    
    ENRICHMENT --> HOT_STORAGE
    ENRICHMENT --> CACHE
    
    HOT_STORAGE --> WARM_STORAGE
    WARM_STORAGE --> COLD_STORAGE
    
    HOT_STORAGE --> ETL
    ETL --> AGGREGATION
    AGGREGATION --> OLAP
    
    CACHE --> API
    HOT_STORAGE --> API
    HOT_STORAGE --> SEARCH
    OLAP --> API
    
    API --> UI
    SEARCH --> UI
    REAL_TIME --> UI
```

## 5. Deployment Architecture

```mermaid
graph TB
    subgraph "Production Environment"
        subgraph "Region 1 - Primary"
            LB1[Load Balancer]
            
            subgraph "Container Orchestration - Kubernetes"
                POD1[Service Pod 1]
                POD2[Service Pod 2]
                POD3[Service Pod 3]
            end
            
            PG1[(PostgreSQL<br/>Primary)]
            REDIS1[(Redis<br/>Primary)]
        end
        
        subgraph "Region 2 - Backup"
            LB2[Load Balancer]
            
            subgraph "Container Orchestration - Kubernetes"
                POD4[Service Pod 1]
                POD5[Service Pod 2]
            end
            
            PG2[(PostgreSQL<br/>Replica)]
            REDIS2[(Redis<br/>Replica)]
        end
    end

    subgraph "Edge Network"
        CDN[CDN<br/>Cloudflare]
        WAF[Web Application Firewall]
    end

    subgraph "Monitoring Stack"
        PROMETHEUS[Prometheus]
        GRAFANA[Grafana]
        ALERT[AlertManager]
    end

    subgraph "CI/CD Pipeline"
        GITHUB[GitHub Actions]
        DOCKER[Docker Registry]
        DEPLOY[Deployment Automation]
    end

    CDN --> WAF
    WAF --> LB1
    WAF --> LB2
    
    LB1 --> POD1
    LB1 --> POD2
    LB1 --> POD3
    
    LB2 --> POD4
    LB2 --> POD5
    
    POD1 --> PG1
    POD2 --> PG1
    POD3 --> PG1
    
    POD1 --> REDIS1
    POD2 --> REDIS1
    POD3 --> REDIS1
    
    PG1 -.Replication.-> PG2
    REDIS1 -.Replication.-> REDIS2
    
    POD1 -.Metrics.-> PROMETHEUS
    POD2 -.Metrics.-> PROMETHEUS
    POD3 -.Metrics.-> PROMETHEUS
    
    PROMETHEUS --> GRAFANA
    PROMETHEUS --> ALERT
    
    GITHUB --> DOCKER
    DOCKER --> DEPLOY
    DEPLOY --> POD1
    DEPLOY --> POD2
    DEPLOY --> POD3
```

## 6. Security Architecture

```mermaid
graph TB
    subgraph "External"
        USER[User/Client]
        ATTACKER[Potential Attacker]
    end

    subgraph "Security Perimeter"
        WAF[Web Application Firewall<br/>- DDoS Protection<br/>- Rate Limiting<br/>- Bot Detection]
        
        GATEWAY[API Gateway<br/>- JWT Validation<br/>- API Key Management<br/>- Request Signing]
    end

    subgraph "Application Security"
        INPUT_VALIDATION[Input Validation<br/>- Schema Validation<br/>- Sanitization<br/>- Type Checking]
        
        AUTH_LAYER[Authentication Layer<br/>- Multi-Factor Auth<br/>- OAuth2<br/>- Session Management]
        
        AUTHZ_LAYER[Authorization Layer<br/>- RBAC<br/>- Resource Permissions<br/>- Data Isolation]
    end

    subgraph "Data Security"
        ENCRYPTION_TRANSIT[Encryption in Transit<br/>- TLS 1.3<br/>- Certificate Pinning]
        
        ENCRYPTION_REST[Encryption at Rest<br/>- Database Encryption<br/>- File Encryption<br/>- Key Management]
        
        DATA_MASKING[Data Masking<br/>- PII Protection<br/>- Audit Logging<br/>- Access Logs]
    end

    subgraph "Monitoring & Response"
        IDS[Intrusion Detection<br/>- Suspicious Activity<br/>- Alert Rules]
        
        AUDIT[Audit Logging<br/>- All Access Logs<br/>- Security Events]
        
        INCIDENT[Incident Response<br/>- Automated Alerts<br/>- Rollback Procedures]
    end

    USER --> WAF
    ATTACKER -.-x WAF
    
    WAF --> GATEWAY
    GATEWAY --> INPUT_VALIDATION
    INPUT_VALIDATION --> AUTH_LAYER
    AUTH_LAYER --> AUTHZ_LAYER
    
    AUTHZ_LAYER --> ENCRYPTION_TRANSIT
    ENCRYPTION_TRANSIT --> ENCRYPTION_REST
    ENCRYPTION_REST --> DATA_MASKING
    
    INPUT_VALIDATION -.-> IDS
    AUTH_LAYER -.-> IDS
    AUTHZ_LAYER -.-> IDS
    DATA_MASKING -.-> AUDIT
    
    IDS --> INCIDENT
    AUDIT --> INCIDENT
```

---

## Architecture Decision Records (ADRs)

### ADR-001: Microservices vs Monolith
**Decision:** Start with a modular monolith, extract services as needed  
**Rationale:** Easier to develop initially, can split later based on scaling needs  
**Consequences:** Single deployment initially, potential complexity when splitting

### ADR-002: Database Strategy
**Decision:** PostgreSQL as primary database with Redis for caching  
**Rationale:** Strong consistency, ACID compliance, excellent JSON support  
**Consequences:** Need to manage connection pooling, implement caching carefully

### ADR-003: Event-Driven Architecture
**Decision:** Use event-driven patterns for async operations  
**Rationale:** Decouples services, enables scalability, improves resilience  
**Consequences:** Need to handle eventual consistency, implement idempotency

### ADR-004: API Design
**Decision:** RESTful APIs with optional GraphQL endpoint  
**Rationale:** REST is well-understood, GraphQL for complex queries  
**Consequences:** Need to maintain both, but provides flexibility

### ADR-005: Authentication Strategy
**Decision:** JWT with refresh tokens + OAuth2  
**Rationale:** Stateless auth, industry standard, supports SSO  
**Consequences:** Token management complexity, need refresh token rotation

### ADR-006: File Storage
**Decision:** S3-compatible storage (AWS S3 or Cloudflare R2)  
**Rationale:** Scalable, cost-effective, CDN integration  
**Consequences:** External dependency, need signed URLs for security

### ADR-007: Caching Strategy
**Decision:** Multi-layer caching (Redis + CDN)  
**Rationale:** Reduces database load, improves response times  
**Consequences:** Cache invalidation complexity, potential stale data

### ADR-008: Testing Strategy
**Decision:** Comprehensive testing pyramid (unit > integration > e2e)  
**Rationale:** Catch bugs early, ensure code quality  
**Consequences:** Longer CI/CD times, test maintenance overhead

---

## Scalability Considerations

### Horizontal Scaling
- All services containerized and stateless
- Load balancing across multiple instances
- Auto-scaling based on metrics

### Database Scaling
- Read replicas for analytics queries
- Connection pooling (PgBouncer)
- Query optimization and indexing
- Partitioning for large tables (expenses by date)

### Caching Strategy
- Application-level caching (in-memory)
- Distributed caching (Redis)
- CDN caching for static assets
- Database query result caching

### Performance Targets
- API Response Time: < 500ms (p95)
- Page Load Time: < 2s
- Database Query Time: < 100ms
- Concurrent Users: 10,000+
- Throughput: 1000 req/s

---

## Disaster Recovery

### Backup Strategy
- Database: Daily full backups, hourly incremental
- File Storage: Multi-region replication
- Configuration: Version controlled
- Recovery Time Objective (RTO): < 4 hours
- Recovery Point Objective (RPO): < 1 hour

### High Availability
- Multi-region deployment
- Database replication
- Health checks and auto-recovery
- Circuit breakers for external services
- Graceful degradation

---

## Cost Optimization

### Infrastructure
- Use serverless where possible (reduce idle costs)
- Auto-scaling to match demand
- Spot instances for batch jobs
- Reserved instances for base load

### Storage
- Lifecycle policies (hot → warm → cold → archive)
- Compress images and files
- CDN for static assets

### Database
- Connection pooling
- Query optimization
- Archive old data
- Use read replicas wisely
