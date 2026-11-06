# High-Level Design (HLD)

## From Understanding Requirements to Designing a Scalable Software System

Theme: Don't start by writing classes or functions. First understand the problem, identify the system boundaries, and design how the major components will work together.

## 1. Opening the classroom – What is HLD?

👨‍🏫 Mentor (Ravi Sir): Good morning, students! Today, we are going to discuss an important engineering skill: High-Level Design (HLD). Let me ask you a simple question. Suppose the government asks you to build a smart city for 10 lakh people. Will you immediately start constructing individual houses?

👦 Student: No, Sir. First, we need to plan roads, water supply, electricity, hospitals, schools and transportation.

👨‍🏫 Mentor: Excellent! Before constructing individual buildings, a city planner prepares a master plan.

That master plan describes:

- What major facilities are required?
- Where should they be located?
- How will they communicate?
- How will people and resources move?
- How will the city handle future growth?

Software engineering follows the same principle. Before implementing individual classes and methods, we design the overall software system. That is called High-Level Design (HLD).

### What is High-Level Design?

High-Level Design is the architectural blueprint of a software system. It describes the major components, their responsibilities, communication mechanisms, data flow, technology choices and deployment architecture.

In simple words:

> HLD explains what major building blocks we need and how those blocks collaborate to solve a business problem.

## 2. HLD vs LLD – Understanding the difference

👨‍🏫 Mentor: In our previous LLD session, we discussed classes, interfaces, SOLID principles and design patterns. Now tell me, what is the difference between HLD and LLD?

👦 Student: Sir, HLD describes the overall architecture, while LLD describes the internal implementation of individual components.

👨‍🏫 Mentor: Absolutely correct!

Think about building a hospital.

- HLD decides the hospital departments, floors, entrances, emergency services and infrastructure.
- LLD decides the room layouts, electrical wiring, doors, switches and equipment placement.

The same relationship exists in software. HLD – Architectural Blueprint

## Complete Software System

Frontend Application

Backend Services

Database System

External Integrations

Components, boundaries, communication and infrastructure

LLD – Internal Component Design

## Backend Service

Classes and Objects

Interfaces

Methods and Logic

Design Patterns

Classes, relationships, algorithms and implementation details

| HLD                          | LLD                            |
| ---------------------------- | ------------------------------ |
| System architecture          | Component implementation       |
| Services and modules         | Classes and objects            |
| Communication protocols      | Method interactions            |
| Database architecture        | Tables and relationships       |
| Deployment architecture      | Internal code structure        |
| Scalability and availability | Algorithms and design patterns |

Remember: HLD decides the city plan; LLD decides how individual buildings are constructed.

## 3. The first step – Understand business requirements

👨‍🏫 Mentor: Students, suppose a customer approaches Transflower and says: "I want to build an online shopping application." Can we immediately start creating React components?

👦 Student: No, Sir. We should understand the requirements first.

👨‍🏫 Mentor: Exactly! A software architect must ask questions before selecting technologies. For an e-commerce application, let's identify the requirements.

### Functional requirements

These describe what the system should do.

1. Customer registration and login.
2. Browse products.
3. Search and filter products.
4. Add products to cart.
5. Place orders.
6. Make payments.
7. Track deliveries.
8. Manage products and inventory.
9. Generate reports.

### Non-functional requirements

These describe how well the system should operate.

| Requirement     | Meaning                                  |
| --------------- | ---------------------------------------- |
| Scalability     | Handle increasing users and transactions |
| Availability    | Keep the application accessible          |
| Performance     | Respond within acceptable time           |
| Security        | Protect users and business data          |
| Reliability     | Process transactions correctly           |
| Maintainability | Make future changes easier               |
| Observability   | Monitor logs, metrics and traces         |
| Fault tolerance | Continue operating despite failures      |

👨‍🏫 Mentor: A good engineer does not just ask, "What features should I develop?" A good engineer also asks, "How many users will use this application? What happens when payment fails? What if the database becomes unavailable?" That is architectural thinking.

## 4. Identify the major components

Let's design a simple e-commerce system. Instead of looking at the application as one large program, divide it into logical components.

### Component identification

| Component              | Responsibility               |
| ---------------------- | ---------------------------- |
| Web Application        | User interface               |
| API Gateway            | Entry point for API requests |
| Authentication Service | Login, registration, tokens  |
| Product Service        | Product catalogue            |
| Cart Service           | Shopping cart                |
| Order Service          | Order processing             |
| Payment Service        | Payment integration          |
| Inventory Service      | Stock management             |
| Notification Service   | Email and SMS                |
| Database               | Persistent storage           |
| Cache                  | Frequently accessed data     |

### HLD architecture diagram

```
                  CUSTOMERS
                      |
                      v
             +------------------+
             |  React Web App   |
             +------------------+
                      |
                      v
             +------------------+
             |   API Gateway    |
             +------------------+
                      |
        +-------------+-------------+
        |             |             |
        v             v             v
  +-----------+ +-----------+ +-----------+
  |   Auth    | |  Product  | |   Cart    |
  |  Service  | |  Service  | |  Service  |
  +-----------+ +-----------+ +-----------+
        |             |             |
        v             v             v
  +-----------+ +-----------+ +-----------+
  | Auth DB   | | Product DB| |  Cart DB  |
  +-----------+ +-----------+ +-----------+

                      |
                      v
              +---------------+
              | Order Service |
              +---------------+
                      |
            +---------+---------+
            |                   |
            v                   v
      +-----------+       +-----------+
      | Inventory |       |  Payment  |
      |  Service  |       |  Service  |
      +-----------+       +-----------+
            |                   |
            v                   v
       Inventory DB        Payment Gateway
```

👨‍🏫 Mentor:

Notice something important. Each component has a responsibility. Product Service manages products. Payment Service manages payment processing. Order Service coordinates order creation. This is called Separation of Concerns. A system becomes easier to understand, develop, test and maintain when responsibilities are clearly separated.

## 5. Understand architectural styles

👨‍🏫 Mentor: Now that we have identified components, should every application use microservices?

👦 Student: No, Sir. It depends on the application requirements.

👨‍🏫 Mentor:Correct! Architecture is not about following trends. It is about making suitable engineering decisions.

### A. Monolithic architecture

All major application modules are deployed as one application.

```
       +-----------------------------+
       |       Web Application       |
       |                             |
       |  +---------+  +----------+  |
       |  | Product |  |   Cart   |  |
       |  +---------+  +----------+  |
       |                             |
       |  +---------+  +----------+  |
       |  |  Order  |  | Payment  |  |
       |  +---------+  +----------+  |
       +-----------------------------+
                     |
                     v
                +---------+
                | Database|
                +---------+
```

Advantages:

- Simple deployment.
- Easy initial development.
- Straightforward debugging.
- Lower infrastructure complexity.

Challenges:

- Large codebase over time.
- Scaling individual modules independently is difficult.
- Deployment changes can affect the entire application.

### B. Layered architecture

Application responsibilities are organized into layers.

```
+-----------------------------+
|       Presentation Layer    |
|       React / Angular       |
+-----------------------------+
               |
               v
+-----------------------------+
|       API / Controller      |
+-----------------------------+
               |
               v
+-----------------------------+
|       Business Layer        |
|       Services              |
+-----------------------------+
               |
               v
+-----------------------------+
|       Data Access Layer     |
|       Repositories          |
+-----------------------------+
               |
               v
+-----------------------------+
|       Database              |
+-----------------------------+
```

Advantages:

- Clear separation of responsibilities.
- Easier testing.
- Suitable for many enterprise applications.

### C. Microservices architecture

An application is divided into independently deployable services.

```
                 API GATEWAY
                      |
        +-------------+-------------+
        |             |             |
        v             v             v
    Product         Order         Payment
    Service         Service       Service
        |             |             |
        v             v             v
    Product DB      Order DB     Payment DB
```

Advantages:

- Independent deployment.
- Independent scaling.
- Technology flexibility.
- Better isolation of certain failures.

Challenges:

- Distributed communication.
- Network failures.
- Data consistency.
- Monitoring and operational complexity.
- More demanding deployment and security management.

### D. Event-driven architecture

Components communicate by publishing and consuming events.

```
           +----------------+
           |  Order Service |
           +----------------+
                   |
                   v
           +----------------+
           | Message Broker |
           | Kafka / Rabbit |
           +----------------+
              /    |     \
             v     v      v
         Inventory Payment Notification
          Service  Service   Service
```

For example: When an order is placed, the Order Service publishes an `OrderCreated` event. Other services subscribe to this event and perform their respective responsibilities.

Mentor's lesson: Choose architecture based on business requirements, team capability, operational maturity and expected scale.

## 6. Understand communication between components

👨‍🏫 Mentor:Students, how does the React application communicate with ASP.NET Core?

👦 Student: Through HTTP APIs, Sir.

👨‍🏫 Mentor: Correct. But in a distributed system, communication can happen in multiple ways.

| Communication style | Technology   | Typical use                        |
| ------------------- | ------------ | ---------------------------------- |
| Synchronous         | REST API     | Request and response               |
| Synchronous         | gRPC         | Efficient service-to-service calls |
| Asynchronous        | RabbitMQ     | Background processing              |
| Asynchronous        | Apache Kafka | Event streaming                    |
| Real-time           | WebSocket    | Live notifications                 |
| Callback            | Webhooks     | External system notifications      |

### Synchronous communication

```
Customer
   |
   v
Order Service
   |
   | HTTP Request
   v
Payment Service
   |
   | Payment Response
   v
Order Service
   |
   v
Customer
```

The caller waits for the response.

### Asynchronous communication

```
Order Service
      |
      | Publish OrderCreated
      v
  Message Broker
      |
      +------------------+
      |                  |
      v                  v
Inventory Service   Notification Service
      |                  |
      v                  v
Update Stock        Send Email
```

The publisher does not need to wait for every consumer to complete its work. Real-world analogy:

- Synchronous communication is like a telephone conversation.
- Asynchronous communication is like sending an email or a letter.

## 7. Database architecture and data management

👨‍🏫 Mentor: Can we put all application data into one table?

👦 Student: No, Sir. We need to organize data according to business entities and relationships.

👨‍🏫 Mentor: Exactly! In HLD, we decide the overall data storage strategy. For our shopping application:

```
+-------------------+
|   Customer DB     |
|-------------------|
| Customers         |
| Addresses         |
+-------------------+

+-------------------+
|   Product DB      |
|-------------------|
| Products          |
| Categories        |
+-------------------+

+-------------------+
|   Order DB        |
|-------------------|
| Orders            |
| Order Items       |
+-------------------+

+-------------------+
|   Payment DB      |
|-------------------|
| Transactions      |
| Payment Status    |
+-------------------+
```

### SQL vs NoSQL

| SQL databases | NoSQL databases |
| ------------- | --------------- |
| MySQL         | MongoDB         |
| PostgreSQL    | Cassandra       |
| SQL Server    | DynamoDB        |
| Oracle        | Redis           |

Choose based on data structure, query patterns, consistency requirements, transaction needs and scale.

Important: Microservices do not automatically require separate physical database servers. A service should own its data and control access to it, but physical deployment can evolve with requirements.

### Caching

Frequently accessed data can be stored in a cache to reduce database load.

```
        Client
           |
           v
       API Server
           |
           v
       Redis Cache
         /     \
     Cache     Cache
     Hit       Miss
       |         |
       v         v
     Return   Database
               |
               v
          Store in Cache
```

Example: Product catalogue information can be cached when appropriate.

## 8. Scalability – How will the system handle growth?

👨‍🏫 Mentor: Imagine our shopping application initially has 100 users. After a successful marketing campaign, 1 lakh users start using it. What happens?

👦 Student: The server may become overloaded, Sir.

👨‍🏫 Mentor: Exactly! We need to think about scalability.

### Vertical scaling

Increase the resources of one server.

```
Before:                  After:

+-----------+            +----------------+
| 2 CPU     |    --->    | 16 CPU         |
| 4 GB RAM  |            | 64 GB RAM      |
+-----------+            +----------------+
```

### Horizontal scaling

Add more servers to handle the workload.

```
                Users
                  |
                  v
            Load Balancer
                  |
          +-------+-------+
          |       |       |
          v       v       v
        App 1   App 2   App 3
          |       |       |
          +-------+-------+
                  |
                  v
             Shared Data
```

| Vertical scaling                    | Horizontal scaling                          |
| ----------------------------------- | ------------------------------------------- |
| Increase server capacity            | Add more server instances                   |
| Simpler initially                   | Requires distributed coordination           |
| Has hardware limits                 | Can grow across machines                    |
| May have a single-server bottleneck | Can improve availability with proper design |

Other scalability techniques:

- Load balancing
- Database indexing
- Read replicas
- Caching
- CDN
- Asynchronous processing
- Database partitioning and sharding when justified

## 9. Availability, reliability and fault tolerance

👨‍🏫 Mentor:  Suppose the Payment Service is unavailable. Should the entire shopping application stop working?

👦 Student: No, Sir. Customers should still be able to browse products.

👨‍🏫 Mentor: Exactly! This is why architects design for failure.

### Common techniques

| Technique           | Purpose                                   |
| ------------------- | ----------------------------------------- |
| Load Balancing      | Distribute incoming requests              |
| Health Checks       | Detect unhealthy instances                |
| Retry               | Attempt a transient failure again         |
| Timeout             | Avoid waiting indefinitely                |
| Circuit Breaker     | Stop repeatedly calling a failing service |
| Replication         | Maintain additional copies of data        |
| Failover            | Switch to a healthy instance              |
| Backup and Recovery | Restore data after failures               |

### Circuit breaker example

```
       Order Service
             |
             v
       Circuit Breaker
             |
             v
       Payment Service
             |
        +----+----+
        |         |
        v         v
      Healthy   Failure
        |         |
        v         v
     Continue   Open Circuit
                   |
                   v
              Fallback /
              Retry Later
```

A circuit breaker typically moves through three states:

1. Closed: Requests flow normally.
2. Open: Requests are blocked temporarily after failures.
3. Half-open: A limited number of test requests are allowed.

## 10. Security architecture

Security should not be an afterthought.

👨‍🏫 Mentor: Suppose a customer logs into our application. Can that customer access the administrator dashboard?

👦 Student: No, Sir. We need authorization.

👨‍🏫 Mentor: Correct! Authentication answers: Who are you? Authorization answers: What are you allowed to do?

### Security architecture

```
             Client Application
                     |
                     v
              API Gateway
                     |
             +-------+-------+
             |               |
             v               v
        Authentication   Authorization
             |               |
             v               v
          JWT Token       RBAC Rules
             |               |
             +-------+-------+
                     |
                     v
              Business APIs
                     |
                     v
                Database
```

Important security considerations:

- OAuth 2.0 / OpenID Connect where appropriate.
- JWT validation.
- Role-Based Access Control (RBAC).
- Encryption in transit using TLS.
- Encryption at rest.
- Secrets management.
- Input validation.
- Rate limiting.
- Audit logging.
- Least-privilege access.

Mentor's principle: Never trust the client application. Validate identity, permissions and business rules on the server.

## 11. Deployment architecture

👨‍🏫 Mentor: We have designed the application components. Now where will we run them?

👦 Student: On servers or cloud infrastructure, Sir.

👨‍🏫 Mentor: Right! An architect must also think about deployment.

### Example deployment architecture

```
                 INTERNET
                     |
                     v
               CDN / WAF
                     |
                     v
              Load Balancer
                     |
                     v
          +----------------------+
          |  Application Layer   |
          |                      |
          |  React Frontend      |
          |  ASP.NET Core APIs   |
          +----------------------+
                     |
          +----------+----------+
          |                     |
          v                     v
      Redis Cache          MySQL Database
                                |
                                v
                          Backup Storage

          Monitoring / Logging / Alerts
```

### Common infrastructure technologies

| Area                    | Technologies                 |
| ----------------------- | ---------------------------- |
| Cloud                   | AWS, Azure, GCP              |
| Containers              | Docker                       |
| Container orchestration | Kubernetes                   |
| CI/CD                   | GitHub Actions, Azure DevOps |
| Monitoring              | Prometheus, Grafana          |
| Logging                 | ELK, OpenSearch              |
| Distributed tracing     | OpenTelemetry                |
| CDN                     | CloudFront, Azure CDN        |

## 12. Observability – Can we understand what is happening?

A system may be running, but that does not mean it is healthy.

For example:

- Why are API requests slow?
- Why are payments failing?
- Which service is consuming more CPU?
- Where did a particular order request fail?

We need observability. The three important pillars are:

- **Logs** : What happened?
- **Metrics** : How much / how often?
- **Traces** : Where did it happen?

Example: A customer places an order. Distributed tracing helps follow the request across the API Gateway, Order Service, Inventory Service and Payment Service.

## 13. Practical HLD case study – Design an Online Examination System

👨‍🏫 Mentor: Now let's apply everything we have learned. Suppose Transflower wants to develop an online examination system. Students should log in, attempt tests, submit answers and view results. Mentors should create assessments and evaluate student performance. How will we design this system?

### Step 1: Identify actors

- Student
- Mentor
- Administrator
- Evaluator

### Step 2: Identify functional requirements

- Student registration and login.
- Assessment creation.
- Question bank management.
- Test scheduling.
- Test attempt.
- Answer submission.
- Automatic evaluation.
- Result generation.
- Performance analytics.

### Step 3: Identify major components

| Component             | Responsibility                   |
| --------------------- | -------------------------------- |
| React Web App         | Student and mentor interface     |
| API Gateway           | Request routing and security     |
| Identity Service      | Authentication and authorization |
| Assessment Service    | Create and manage assessments    |
| Question Bank Service | Store questions                  |
| Exam Engine           | Manage test attempts             |
| Evaluation Service    | Evaluate answers                 |
| Reporting Service     | Generate reports                 |
| MySQL                 | Persistent storage               |
| Redis                 | Cache and temporary state        |

### Step 4: HLD diagram

```
                STUDENT / MENTOR
                       |
                       v
                +--------------+
                | React Client |
                +--------------+
                       |
                       v
                +--------------+
                | API Gateway  |
                +--------------+
                       |
       +---------------+----------------+
       |               |                |
       v               v                v
  +----------+   +------------+   +------------+
  | Identity |   | Assessment |   | Question   |
  | Service  |   | Service    |   | Bank       |
  +----------+   +------------+   +------------+
       |               |                |
       v               v                v
    MySQL            MySQL             MySQL

                       |
                       v
                +--------------+
                |  Exam Engine |
                +--------------+
                       |
                       v
                +--------------+
                |  Evaluation  |
                |   Service    |
                +--------------+
                       |
                       v
                +--------------+
                |  Reporting   |
                |   Service    |
                +--------------+
                       |
                       v
                  Analytics
```

### Step 5: Test submission workflow

```
Student
   |
   | Start Exam
   v
Exam Engine
   |
   | Fetch Questions
   v
Question Bank
   |
   | Questions
   v
Student
   |
   | Submit Answers
   v
Exam Engine
   |
   | Submission Event
   v
Evaluation Service
   |
   | Calculate Score
   v
Result Database
   |
   v
Reporting Service
   |
   v
Student Dashboard
```

### Step 6: Discuss important design decisions

👨‍🏫 Mentor: What happens if a student clicks Submit twice?

👦 Student: We should prevent duplicate submissions.

👨‍🏫 Mentor: Correct. We can use idempotency controls and enforce submission rules. What happens if the network disconnects during the examination?

👦 Student: We should save answers periodically and allow recovery according to exam rules.

👨‍🏫 Mentor: Excellent! What if 5,000 students submit answers at the same time?

👦 Student: We need to handle the load through scalable APIs, database optimization and possibly asynchronous evaluation.

This is HLD thinking: identify the problem first, then design the solution.

## 14. HLD design process – A systematic approach

Whenever you receive a system design problem, follow this sequence.

```
       +-------------------------+
       | 1. Understand Problem   |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 2. Gather Requirements  |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 3. Estimate Scale       |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 4. Identify Components |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 5. Define Data Model    |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 6. Define Communication |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 7. Design Infrastructure|
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 8. Address NFRs         |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 9. Review Trade-offs    |
       +-------------------------+
                   |
                   v
       +-------------------------+
       | 10. Document HLD        |
       +-------------------------+
```

### Important HLD deliverables

| Deliverable          | What it communicates                        |
| -------------------- | ------------------------------------------- |
| Requirement document | Business and system expectations            |
| Context diagram      | System boundaries and external actors       |
| Component diagram    | Major system building blocks                |
| Architecture diagram | Overall architecture                        |
| Data architecture    | Storage and data ownership                  |
| Sequence diagram     | Inter-component interactions                |
| Deployment diagram   | Infrastructure and hosting                  |
| ADR                  | Architecture Decision Records               |
| NFR matrix           | Performance, security, availability targets |

## 15. HLD interview preparation

Students, interviewers may ask you to design systems such as:

| System             | Important concepts                       |
| ------------------ | ---------------------------------------- |
| URL Shortener      | Hashing, caching, database               |
| WhatsApp           | Messaging, WebSockets, delivery          |
| YouTube            | Video storage, CDN, streaming            |
| Uber               | Geolocation, matching, real-time updates |
| Amazon             | Product catalogue, cart, order, payment  |
| BookMyShow         | Seat locking, concurrency, payment       |
| Banking System     | Transactions, consistency, security      |
| Online Examination | Test attempts, evaluation, reporting     |

When asked to design a system, don't jump directly into microservices or technology selection.

Start with the problem, requirements, assumptions and constraints.
 
A record of an architectural decision and its rationale
 

## 16. Transflower practical assignment

Classroom assignment

Problem Statement: Design a Food Delivery Application

Your task is to prepare an HLD for an application similar to a food delivery platform.

### Expected features

1. Customer registration and login.
2. Restaurant discovery.
3. Menu browsing.
4. Cart management.
5. Order placement.
6. Payment processing.
7. Delivery partner assignment.
8. Live order tracking.
9. Restaurant dashboard.
10. Notifications and ratings.

### Submission requirements

- Functional and non-functional requirements.
- System context diagram.
- Component architecture diagram.
- Database architecture.
- Communication design.
- Deployment architecture.
- Security architecture.
- Scalability and availability strategy.
- At least three Architecture Decision Records.

### Assessment rubric

| Evaluation criteria               | Marks |
| --------------------------------- | ----- |
| Requirement analysis              | 15    |
| Component identification          | 15    |
| Architecture diagram              | 20    |
| Data and communication design     | 15    |
| Scalability and reliability       | 15    |
| Security and deployment           | 10    |
| Design explanation and trade-offs | 10    |
| Total                             | 100   |

## 18. Closing mentor message

👨‍🏫 Ravi Sir: 

Students, remember one important thing. A programmer thinks about writing code. A software designer thinks about organizing code. But a software architect thinks about how the entire system will work.

When you design a system, you must think about:

- Business requirements.
- Users and their expectations.
- Components and responsibilities.
- Communication and data flow.
- Performance and scalability.
- Security and reliability.
- Deployment and operations.
- Future maintenance and evolution.

One more thing! 

Don't become obsessed with microservices, Kubernetes, Kafka or cloud technologies. First understand the problem. Then identify the responsibilities. Then design the architecture. Finally, select the appropriate technologies. Technology follows architecture. Architecture follows requirements. Requirements follow business needs.

```
Business Problem
      |
      v
Requirement Analysis
      |
      v
High-Level Design (HLD)
      |
      v
Low-Level Design (LLD)
      |
      v
Implementation
      |
      v
Testing
      |
      v
Deployment
      |
      v
Monitoring and Evolution
```

A storm does not test the leaves; it tests the roots. Similarly, a complex software project does not test only your coding skills. It tests your engineering fundamentals.

At Transflower, our goal is not to produce framework operators. Our goal is to develop engineers who can understand, design, build and evolve real-world software systems.
