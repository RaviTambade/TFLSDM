# System Design Thinking

Theme: Don't just think about writing code. Think about designing a system that can serve people reliably.

Mentor's analogy: A System Designer is like a City Planner.

# System Design Thinking: From City Planning to Software Architecture

**Mentor:** Ravi Sir\
**Mentee:** Om\
**Theme:** Understand system design through real-world infrastructure and engineering trade-offs.

## 1. Opening: Think Like a City Planner

 **Mentor:**: Om, today we are going to discuss System Design. Before talking about software, imagine that you are a city planner. You have been given responsibility for designing a city for millions of people. Tell me, if a city is poorly designed, what happens?

**Om:**: Sir, there will be traffic jams, electricity problems, water shortages and overcrowding. Basically, chaos!

**Mentor:** Exactly! Now think about software. If an application is poorly designed, what happens?

**Om:** Users may experience slow response times, application crashes, database failures and poor user experience.

**Mentor:** Correct! A city needs roads, electricity, water supply, communication networks and public services. Similarly, a software system needs networking, storage, compute, communication and architecture.

**System Design is the process of deciding how these building blocks work together to solve a business problem.**

Let's understand them one by one.
 

## 2. Networking – Roads and Traffic

**Mentor:** Om, imagine a person wants to visit a building called Google. How does the browser know where `google.com` lives?

**Om:** Through DNS, Sir. It converts the domain name into an IP address.

**Mentor:** Excellent! DNS is like a city directory. It helps locate the destination. Now imagine millions of people trying to enter the same building. What happens if all requests reach one server?

**Om:** The server may become overloaded.

**Mentor:** Right! We need a traffic controller. That is where a **Load Balancer** comes in. It distributes incoming requests across multiple servers. And what if users are located in different countries?

**Om:** We can use a CDN to deliver content from locations closer to users.

**Mentor:** Very good!

| Networking Term | City Analogy               | Responsibility                          |
| --------------- | -------------------------- | --------------------------------------- |
| DNS             | City directory             | Resolves domain names to IP addresses   |
| IP Address      | Building address           | Identifies a network destination        |
| HTTP/HTTPS      | Communication rules        | Enables web communication               |
| Load Balancer   | Traffic controller         | Distributes incoming requests           |
| CDN             | Local distribution centers | Delivers cached content closer to users |
| Firewall        | Security checkpoint        | Controls network traffic                |

**Mentor's point:** Roads help people reach places. Networking helps requests reach services.

 
## 3. Storage – Granaries and Libraries

**Mentor:** Om, suppose you are designing Instagram. Users upload millions of photographs. Would you store all those photographs directly inside SQL database tables?

**Om:** No, Sir. SQL databases are useful for structured information. Photographs can be stored in Object Storage such as Amazon S3.

**Mentor:** Correct! Think of a database as a library catalogue. It stores information about the books. Object Storage is like the warehouse where the actual books are kept. For example:

- User information → Relational Database
- Photo metadata → Relational Database
- Actual photographs → Object Storage
- Frequently accessed feed data → Cache

Now tell me, what is caching?

**Om:** Caching means keeping frequently accessed data in faster storage so that we don't have to repeatedly access the database.

**Mentor:** Exactly! Redis is a popular in-memory data store used for caching.

| Storage Term   | City Analogy                 | Responsibility                      |
| -------------- | ---------------------------- | ----------------------------------- |
| SQL Database   | Organized library            | Stores structured, relational data  |
| NoSQL Database | Flexible record archive      | Supports different data models      |
| Object Storage | Large warehouse              | Stores images, videos and documents |
| Redis Cache    | Frequently used records desk | Provides fast access to cached data |
| Database Index | Library catalogue            | Helps locate records efficiently    |

**Mentor's point:** Don't store everything in the same place. Choose storage according to the nature of the data and access pattern.

## 4. Compute – Factories

**Mentor:** Now imagine that our city has factories where products are manufactured. In software, where does application processing happen?

**Om:**On compute infrastructure, Sir. Such as virtual machines, containers or serverless platforms.

**Mentor:**Good! Let's understand the differences.

**Virtual Machine (VM)**

A VM is like an independent factory building with its own operating system.

**Container**

A container is like a production unit operating inside a shared industrial facility. Containers share the host operating system kernel while isolating application processes and dependencies.

**Serverless**

Serverless is like hiring a production facility only when a particular job arrives. The cloud provider manages the underlying server infrastructure.

| Compute Term    | City Analogy                 | Responsibility                                |
| --------------- | ---------------------------- | --------------------------------------------- |
| Physical Server | Factory building             | Provides computing resources                  |
| Virtual Machine | Independent factory          | Runs an operating system and applications     |
| Container       | Isolated production unit     | Packages an application and its dependencies  |
| Kubernetes      | Factory coordinator          | Orchestrates container deployment and scaling |
| Serverless      | On-demand production service | Runs code without managing servers directly   |

**Mentor's point:** Compute is where your software performs its work.

## 5. Communication – Post and Telephone Networks

**Mentor:** Om, imagine WhatsApp. When two people exchange messages, what kind of communication do they need?

**Om:** Real-time communication, Sir. WebSockets can provide a persistent, two-way connection.

**Mentor:** Correct! Now suppose an order service receives an order, but sending an email notification takes some time. Should the order service wait until the email is delivered?

**Om:** Not necessarily. It can publish a message to a queue. Another service can process it asynchronously.

**Mentor:** Exactly! That is the idea behind Message Queues.

| Communication Term | City Analogy                     | Responsibility                             |
| ------------------ | -------------------------------- | ------------------------------------------ |
| REST API           | Formal service counter           | Request-response communication             |
| WebSocket          | Telephone connection             | Real-time two-way communication            |
| Message Queue      | Post office                      | Asynchronous message delivery              |
| Event Streaming    | News distribution network        | Continuous distribution of events          |
| gRPC               | High-speed communication channel | Efficient service-to-service communication |

**Mentor's point:** Services should communicate using the mechanism that fits their requirements.

 
## 6. Architecture Patterns – City Layouts

**Mentor:** Now comes an important question. How should we organize all these buildings, roads, factories and services? That is an architecture decision. Tell me, Om, what is a Monolithic Architecture?

**Om:**Sir, a monolithic application is built and deployed as one application unit. Its modules can still be logically separated.

**Mentor:**Very good! And Microservices?

**Om:** The application is divided into independently deployable services. Each service owns a specific business capability and communicates with other services through defined contracts.

**Mentor:** Excellent!

Let's compare.

| Architecture              | City Analogy                          | Characteristics                                      |
| ------------------------- | ------------------------------------- | ---------------------------------------------------- |
| Monolith                  | One integrated city complex           | Application deployed as one unit                     |
| Modular Monolith          | One complex with separate departments | Clear internal module boundaries                     |
| Microservices             | Multiple specialized buildings        | Independently deployable business services           |
| Event-Driven Architecture | City-wide notification network        | Components communicate through events                |
| Serverless Architecture   | On-demand public services             | Managed infrastructure and event-triggered execution |

**Important discussion:**

A startup does not automatically need microservices. For a small application, a well-designed modular monolith may be easier to build, test, deploy and maintain. As the system grows, independently scaling or deploying certain capabilities may justify microservices. Architecture is not about choosing the most fashionable technology. It is about choosing the right structure for the problem.

## 7. System Design Is About Trade-offs

**Mentor:** Om, imagine that you are designing an application for 100 users. Would you immediately introduce:

- 20 microservices?
- Multiple Kubernetes clusters?
- Distributed databases?
- Global CDN infrastructure?

**Om:** No, Sir. That would introduce unnecessary complexity and operational costs.

**Mentor:** Exactly! Now imagine the application grows to millions of users. We may need to consider:

- Load balancing
- Horizontal scaling
- Distributed caching
- Database replication
- Database partitioning or sharding
- Asynchronous processing
- Monitoring and observability

But remember: user count alone does not determine architecture. Workload, traffic patterns, data volume, availability requirements, latency and business constraints matter too.

**System Design = Requirements + Architecture + Technology Choices + Trade-offs.**

## 8. A Practical Example: Transflower Assessment Platform

Let's design a simplified Student Assessment Management System.

**Business requirement:**

Thousands of students should be able to log in, attempt assessments and view their results.

A possible high-level architecture:

```
                  STUDENTS / MENTORS
                          |
                          v
                  +---------------+
                  |  Web / Mobile |
                  |    Client     |
                  +---------------+
                          |
                          v
                  +---------------+
                  | Load Balancer |
                  +---------------+
                          |
                          v
                  +---------------+
                  | ASP.NET Core  |
                  |   API Layer   |
                  +---------------+
                          |
              +-----------+-----------+
              |           |           |
              v           v           v
        +-----------+ +---------+ +----------+
        | Identity  | |Assessment| | Results  |
        | Module    | | Module   | | Module   |
        +-----------+ +---------+ +----------+
              |           |           |
              +-----------+-----------+
                          |
                          v
                  +---------------+
                  |   MySQL DB    |
                  +---------------+
                          |
                          v
                  +---------------+
                  | Redis Cache   |
                  +---------------+

        Asynchronous Processing
                  |
                  v
             +----------+
             | Message  |
             |  Queue   |
             +----------+
                  |
             +----+------+
             |           |
             v           v
        +----------+ +----------+
        | Email    | | Reports  |
        | Service  | | Service  |
        +----------+ +----------+
```

This is an illustrative starting architecture, not a requirement to implement every component immediately.
 

## 9. Closing Conversation

**Mentor:**Om, what did you learn today?

**Om:**Sir, System Design is not about memorizing technology names. 

It is about understanding how different components work together. 
- Networking handles requests.
- Storage manages data.
- Compute executes applications.
- Communication connects services.
- Architecture organizes the system.

**Mentor:** Excellent!  And what is the most important skill of a System Designer?

**Om:** Choosing the right solution by understanding requirements, constraints and trade-offs.

 **Mentor:** Absolutely! Remember these principles:

1. Start with the business problem.
2. Understand functional and non-functional requirements.
3. Estimate the expected workload and scale.
4. Design components and their responsibilities.
5. Select appropriate technologies.
6. Identify bottlenecks and failure scenarios.
7. Evaluate cost, security, performance and maintainability.
8. Evolve the architecture when the business needs it.

 
## Final Mentor Message

> Don't become a technology collector. Become a problem solver.
>
> A good programmer knows how to write code.
>
> A good software engineer knows how to build reliable applications.
>
> A good system designer knows how to make multiple components work together at scale.

**System Design is not about using more technology. It is about making better engineering decisions.**

— Ravi Sir | Transflower Mentoring

## Classroom activity: Design before coding

Ask students to design a simple online examination system for 1,000 concurrent students.

Each team must explain:

1. How will users connect to the application?
2. Where will student and assessment data be stored?
3. How will the system handle simultaneous exam submissions?
4. How will results be calculated?
5. What happens if the database becomes unavailable?
6. Which components need caching or asynchronous processing?

Mentor's challenge: Before writing a single line of code, draw the architecture and explain why each component exists.