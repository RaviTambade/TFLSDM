# System Design in Practice

## Designing URL Shortener, WhatsApp and BookMyShow

Theme: Learn how to convert a business problem into a scalable software architecture.

Ravi Sir: Good morning, students! In our previous session, we learned High-Level Design (HLD). We discussed components, services, databases, caching, scalability, availability and deployment.

Today, we are going to apply those concepts. I will give you three real-world problems:

1. Design a URL Shortener like Bitly.
2. Design a messaging application like WhatsApp.
3. Design a movie-ticket booking application like BookMyShow.

But before starting, remember one engineering principle:

> Don't begin system design by selecting technologies. Begin by understanding the problem.

## 1. The system design approach

Suppose a customer asks you to develop an application. What questions should you ask before designing it?

Student: Sir, we should understand the features, number of users, expected traffic and business requirements.

Ravi Sir: Exactly! We will follow this process for every system design problem.

```
       BUSINESS PROBLEM
              |
              v
       REQUIREMENT ANALYSIS
              |
              v
       FUNCTIONAL REQUIREMENTS
              |
              v
       NON-FUNCTIONAL REQUIREMENTS
              |
              v
       CAPACITY ESTIMATION
              |
              v
       COMPONENT DESIGN
              |
              v
       DATABASE DESIGN
              |
              v
       COMMUNICATION DESIGN
              |
              v
       SCALABILITY & RELIABILITY
              |
              v
       ARCHITECTURE DECISIONS
```

### Two important requirement categories

| Functional requirements    | Non-functional requirements |
| -------------------------- | --------------------------- |
| What should the system do? | How well should it work?    |
| Login                      | Security                    |
| Send message               | Low latency                 |
| Book ticket                | Data consistency            |
| Upload video               | Availability                |
| Generate report            | Scalability                 |

# Case Study 1: Design a URL Shortener

 

Business problem

Create a service that converts long URLs into short URLs and redirects users to the original destination.

Examples:

```
Original URL:
https://www.example.com/products/electronics/mobile/iphone

Short URL:
https://short.ly/a8X92k
```

Ravi Sir:

Students, what is the main responsibility of this system?

Student: Sir, it should generate a unique short code and redirect users to the original URL.

Ravi Sir: Correct. Let's design it.

## 1.1 Functional requirements

- Accept a long URL.
- Generate a unique short code.
- Store the mapping.
- Redirect short URL requests.
- Track click analytics.
- Optionally support expiration and custom aliases.

## 1.2 Non-functional requirements

- Redirect should be fast.
- Short codes must be unique.
- Service should support high read traffic.
- Service should be highly available.
- Data should not be lost.

## 1.3 Capacity estimation

Assume:

- 10 million new short URLs per month.
- 100 million redirects per month.
- Average mapping record size: approximately 500 bytes.

Approximate storage growth:

\\[ 10,000,000 \times 500 = 5,000,000,000 \text{ bytes} \\]

That is approximately 5 GB per month, before indexes, replication and overhead.

Read/write ratio:

\\[ \frac{100M}{10M}=10:1 \\]

This is a read-heavy system.

Architectural conclusion: Caching is especially valuable because redirects happen more frequently than URL creation.

## 1.4 Component architecture

```
                 USER
                  |
                  v
           +--------------+
           | Web / Mobile |
           +--------------+
                  |
                  v
           +--------------+
           | API Gateway  |
           +--------------+
                  |
          +-------+-------+
          |               |
          v               v
    +-----------+   +-------------+
    | URL Create|   | URL Redirect|
    | Service   |   | Service     |
    +-----------+   +-------------+
          |               |
          v               v
      +--------+      +--------+
      |Database|<---->| Redis  |
      +--------+      +--------+
                          |
                          v
                    Original URL
```

## 1.5 Short-code generation

How do we generate a short code? One approach is to use a unique numeric ID and encode it using Base62. Base62 uses:

```
0-9  = 10 characters
a-z  = 26 characters
A-Z  = 26 characters

Total = 62 characters
```

For example:

```
Unique ID: 125789

Base62 Encoding

125789  ------>  "W7x"
```

The actual code depends on the encoding algorithm and ID. Other approaches include random identifiers and carefully managed distributed ID generators.

Important: Random codes need collision detection. A database uniqueness constraint is still essential.

## 1.6 Database design

URLMappings

| Column      | Type        | Purpose           |
| ----------- | ----------- | ----------------- |
| Id          | BIGINT      | Primary key       |
| ShortCode   | VARCHAR(20) | Unique identifier |
| OriginalUrl | TEXT        | Destination URL   |
| CreatedAt   | DATETIME    | Creation time     |
| ExpiresAt   | DATETIME    | Optional expiry   |

Example:

```
URLMappings
---------------------------------
Id       ShortCode   OriginalUrl
---------------------------------
101      a8X92k      example.com/page1
102      b7Y31m      example.com/page2
103      c9P45n      example.com/page3
```

## 1.7 Request flow

URL creation

1. Client sends original URL.
2. URL service validates the input.
3. Generate a unique short code.
4. Save the mapping in the database.
5. Return the short URL.

URL redirection

1. Browser requests short URL.
2. Redirect service checks Redis cache.
3. On cache miss, query the database.
4. Return HTTP redirect to the original URL.
5. Record click analytics asynchronously if required.

### Architecture decisions

| Problem               | Possible solution             |
| --------------------- | ----------------------------- |
| Duplicate short codes | Unique constraint and retry   |
| High redirect traffic | Redis cache                   |
| Slow analytics        | Asynchronous event processing |
| Expired URLs          | Expiry validation and cleanup |
| Database growth       | Partitioning or archival      |

Mentor's takeaway: URL Shortener is a read-heavy system. Design the redirect path for low latency.

# Case Study 2: Design WhatsApp – Real-Time Messaging System

Business problem

Design a messaging application that allows users to send and receive messages, share media and see delivery status in real time.

Ravi Sir: Now imagine 10 lakh students are using our messaging application. Om sends a message to Riya. What should happen?

Om: Sir, my message should reach Riya's mobile.

Ravi Sir: Correct. But what if Riya is offline? What if her internet connection is disconnected? What if she has multiple devices? Now you are thinking like a system designer!

## 2.1 Functional requirements

- User registration and authentication.
- One-to-one messaging.
- Group messaging.
- Message delivery and read receipts.
- Online/offline presence.
- Image, video and document sharing.
- Message history.
- Push notifications.

## 2.2 Non-functional requirements

- Low message delivery latency.
- High availability.
- Reliable message delivery.
- Secure communication.
- Support for millions of concurrent connections.
- Scalable media storage.

## 2.3 Major architectural components

| Component              | Responsibility                 |
| ---------------------- | ------------------------------ |
| Mobile/Web Client      | Chat interface                 |
| API Gateway            | Entry point and routing        |
| Authentication Service | User identity                  |
| Connection Manager     | Manage real-time connections   |
| Messaging Service      | Process messages               |
| Message Broker         | Distribute events              |
| Presence Service       | Online/offline status          |
| Notification Service   | Push notifications             |
| Media Service          | Upload and retrieve media      |
| Message Database       | Persist messages               |
| Object Storage         | Store images, videos and files |

## 2.4 HLD architecture

```
                 USERS
                   |
          +--------+--------+
          |                 |
          v                 v
      Mobile App         Web App
          |                 |
          +--------+--------+
                   |
                   v
             API Gateway
                   |
          +--------+---------+
          |                  |
          v                  v
   Authentication       Connection
      Service            Manager
                             |
                             v
                       WebSocket
                             |
                             v
                      Messaging Service
                             |
                     +-------+-------+
                     |               |
                     v               v
                Message Broker   Message DB
                     |
           +---------+----------+
           |                    |
           v                    v
      Presence Service    Notification Service
           |                    |
           v                    v
       Redis Cache          Push Provider

          Media Service
                |
                v
          Object Storage
```

## 2.5 Why WebSocket?

Ravi Sir: Suppose Om sends a message. Should the mobile application continuously ask the server: "Any new message? Any new message? Any new message?"

Student: That would create too many requests, Sir.

Ravi Sir: Exactly! Traditional polling repeatedly requests updates. WebSocket provides a persistent, bidirectional connection between client and server. HTTP request-response

```
Client  ---- Request ----> Server
Client  <--- Response ---- Server
```

Each interaction follows the request-response pattern.

WebSocket communication

```
Client  <==== Persistent connection ====> Server

Client  <-------- Message -------------- Server
Client  --------- Message -------------> Server
Client  <-------- Status --------------- Server
```

Both sides can send data over the established connection. WebSocket is useful for real-time chat, collaboration and live dashboards.

## 2.6 Message delivery workflow

Suppose Om sends "Hello Riya".

```
       Om's Mobile
            |
            | Send Message
            v
      Connection Manager
            |
            v
      Messaging Service
            |
            +-------> Message Database
            |
            v
        Message Broker
            |
            v
      Riya's Connection
            |
            v
       Riya's Mobile
            |
            | Delivery Acknowledgement
            v
      Messaging Service
            |
            v
        Update Status
```

### Message states

| State     | Meaning                          |
| --------- | -------------------------------- |
| Sent      | Server has accepted the message  |
| Delivered | Recipient device has received it |
| Read      | Recipient has opened or read it  |

These states must be clearly defined in the system. A server accepting a message does not necessarily mean the recipient has received it.

## 2.7 What if Riya is offline?

Ravi Sir: Suppose Riya switches off her phone. Should Om lose his message?

Student: No, Sir. We should store it and deliver it when Riya comes online.

Ravi Sir: Correct! This requires durable message storage and an offline-delivery mechanism.

```
       Om sends message
              |
              v
       Messaging Service
              |
              v
        Message Database
              |
              v
      Check Recipient Status
              |
        +-----+-----+
        |           |
      Online      Offline
        |           |
        v           v
     Deliver    Keep Pending
        |           |
        |           v
        |       Riya reconnects
        |           |
        +-----------+
              |
              v
       Deliver Pending Message
```

For reliability, we also need:

- Message IDs.
- Acknowledgements.
- Retry policies.
- Idempotent processing.
- Ordering rules.
- Durable storage.
- Delivery-state tracking.

Important: Distributed systems can deliver events more than once. Consumers must be prepared to handle duplicates safely.

## 2.8 Database architecture

A simplified relational model:

```
USERS
-----
UserId
Name
PhoneNumber

CONVERSATIONS
-------------
ConversationId
ConversationType
CreatedAt

CONVERSATION_MEMBERS
--------------------
ConversationId
UserId

MESSAGES
--------
MessageId
ConversationId
SenderId
MessageBody
CreatedAt
MessageStatus

MEDIA
-----
MediaId
MessageId
StorageLocation
MediaType
```

At very large scale, message storage may need partitioning by conversation or another access pattern. The database choice should follow message volume, ordering, retention and query requirements.

## 2.9 Media architecture

Should we store a 200 MB video directly inside the message database?

Student: No, Sir. We should use object storage.

Ravi Sir: Exactly!

```
        Mobile Client
             |
             v
         Media API
             |
             v
      Object Storage
      (Images / Video)
             |
             v
        CDN Delivery
             |
             v
       Recipient App
```

The message database stores metadata and a reference to the media object. The actual media file is stored separately.

Mentor's takeaway: Messaging is a real-time, connection-heavy and reliability-sensitive system. Design for online and offline users.

# Case Study 3: Design BookMyShow – Movie Ticket Booking System

Business problem

Design an online ticket booking platform where customers can discover movies, select seats, pay and receive confirmed tickets.

Ravi Sir: Students, here is an interesting problem. A cinema hall has 100 seats. Two customers open the application at the same time and select seat A10. Can we allow both customers to book A10?

Student: No, Sir. The same seat should not be booked twice.

Ravi Sir: Excellent! This is the central design challenge: concurrency control and consistency.

## 3.1 Functional requirements

- Browse movies.
- Search cinemas and showtimes.
- View seat availability.
- Select seats.
- Temporarily hold selected seats.
- Process payment.
- Confirm booking.
- Generate tickets.
- Cancel bookings and process refunds.

## 3.2 Non-functional requirements

- Prevent double booking.
- Handle high traffic during popular releases.
- Provide reliable payment processing.
- Maintain correct booking status.
- Support fast seat availability updates.
- Ensure secure transactions.

## 3.3 Major components

| Component               | Responsibility                |
| ----------------------- | ----------------------------- |
| Customer Application    | Movie and booking UI          |
| API Gateway             | Route requests                |
| Movie Catalogue Service | Movie information             |
| Theatre Service         | Cinema and screen information |
| Show Service            | Showtimes                     |
| Seat Inventory Service  | Seat availability and locking |
| Booking Service         | Booking lifecycle             |
| Payment Service         | Payment processing            |
| Ticket Service          | Ticket generation             |
| Notification Service    | SMS/email confirmation        |

## 3.4 HLD architecture

```
                  CUSTOMERS
                      |
                      v
              +---------------+
              | React / Mobile|
              +---------------+
                      |
                      v
                +-----------+
                | API Gateway|
                +-----------+
                      |
       +--------------+----------------+
       |              |                |
       v              v                v
  +----------+   +----------+    +-------------+
  | Movie    |   | Show     |    | Theatre     |
  | Service  |   | Service  |    | Service     |
  +----------+   +----------+    +-------------+
       |              |                |
       +--------------+----------------+
                      |
                      v
             +------------------+
             | Seat Inventory   |
             | Service          |
             +------------------+
                      |
                      v
              +---------------+
              | Booking Service|
              +---------------+
                      |
             +--------+--------+
             |                 |
             v                 v
        Payment Service   Ticket Service
             |                 |
             v                 v
       Payment Gateway    Ticket Store
                               |
                               v
                       Notification Service
```

## 3.5 Seat selection and temporary locking

Ravi Sir: Let's understand the seat lifecycle. A seat can be:

- Available
- Temporarily held
- Booked

```
       +-----------+
       | Available |
       +-----------+
             |
             | Customer selects seat
             v
       +-----------+
       |   Held    |
       +-----------+
          /     \
         /       \
   Payment      Hold expires /
   succeeds     payment fails
      |                |
      v                v
 +----------+     +-----------+
 |  Booked  |     | Available |
 +----------+     +-----------+
```

A temporary hold might last for a few minutes, depending on business policy.

## 3.6 How do we prevent double booking?

This is where database transactions become important. Consider two users:

```
Customer A selects A10
Customer B selects A10
          |
          v
     Booking API
          |
          v
     Seat Inventory
          |
          v
  Atomic Seat Reservation
          |
     +----+----+
     |         |
     v         v
   User A    User B
   Success   Rejected
```

Possible implementation techniques:

1. Database transaction with row-level locking.
2. Atomic conditional update.
3. Unique constraints on active bookings.
4. Optimistic concurrency using version numbers.
5. Distributed locking, only when justified and carefully designed.

For example, a relational database could perform an atomic update:

```
UPDATE Seats
SET Status = 'HELD',
    HoldExpiresAt = CURRENT_TIMESTAMP + INTERVAL 5 MINUTE
WHERE ShowId = 501
  AND SeatId = 'A10'
  AND Status = 'AVAILABLE';
```

The application checks the number of affected rows:

- One row updated: seat successfully held.
- Zero rows updated: seat is no longer available.

This operation must be executed with appropriate transaction and expiry-handling logic.

Mentor's lesson: A seat must not be reserved merely because it appeared available on the screen. The server must confirm it atomically.

## 3.7 Payment and booking workflow

Now comes another important challenge. What if the customer pays successfully, but the booking service does not receive the payment response?

Student: Sir, we need to verify payment status before confirming the ticket.

Ravi Sir: Correct! We should not rely only on the browser's success page.

```
Customer
   |
   | Select Seats
   v
Seat Inventory
   |
   | Hold Seats
   v
Booking Service
   |
   | Create Pending Booking
   v
Payment Service
   |
   | Initiate Payment
   v
Payment Gateway
   |
   | Payment Callback / Webhook
   v
Payment Verification
   |
   +--------------------+
   |                    |
   v                    v
Payment Success    Payment Failure
   |                    |
   v                    v
Confirm Booking    Release Seats
   |
   v
Generate Ticket
   |
   v
Send Notification
```

Important engineering decisions:

- Use idempotency keys for payment requests.
- Verify payment through trusted server-side mechanisms.
- Handle delayed or duplicate webhooks.
- Reconcile payment and booking status.
- Release expired seat holds.
- Make refund processing a separate controlled workflow.

A robust implementation may use a Saga pattern to coordinate booking, payment and inventory changes across services.

## 3.8 Database design

A simplified relational model:

```
MOVIES
------
MovieId
Title
Language
Duration

THEATRES
--------
TheatreId
Name
Location

SCREENS
-------
ScreenId
TheatreId
ScreenName

SHOWS
-----
ShowId
MovieId
ScreenId
ShowTime

SEATS
-----
SeatId
ScreenId
SeatNumber
SeatType

BOOKINGS
--------
BookingId
UserId
ShowId
BookingStatus
TotalAmount

BOOKING_SEATS
-------------
BookingId
SeatId

PAYMENTS
--------
PaymentId
BookingId
TransactionId
PaymentStatus
Amount
```

The design must also enforce that the same seat cannot have two active bookings for the same show.

Mentor's takeaway: BookMyShow is a concurrency-sensitive and transaction-heavy system. Data correctness is more important than merely displaying a fast UI.

# 4. Compare the three architectures

Ravi Sir: Students, notice something interesting. We used the same HLD methodology, but the architecture changed because the business problems were different.

| Design dimension    | URL Shortener        | WhatsApp            | BookMyShow             |
| ------------------- | -------------------- | ------------------- | ---------------------- |
| Primary challenge   | Fast redirects       | Real-time messaging | Prevent double booking |
| Workload            | Read-heavy           | Connection-heavy    | Transaction-heavy      |
| Communication       | HTTP                 | WebSocket + events  | HTTP + events          |
| Storage             | URL mapping          | Messages and media  | Bookings and seats     |
| Caching             | Redis                | Redis               | Redis                  |
| Important concept   | Base62               | Message delivery    | Concurrency control    |
| Reliability concern | Mapping availability | Message durability  | Booking consistency    |

## 5. How to estimate system capacity

One important skill for system designers is capacity estimation. Let's consider a hypothetical system with 1 million daily active users. Assume each user generates 100 API requests per day.

Total daily requests:

\\[ 1,000,000 \times 100 = 100,000,000 \\]

Average requests per second:

\\[ \frac{100,000,000}{86,400} \approx 1,157 \\]

If peak traffic is five times the average:

\\[ 1,157 \times 5 = 5,785 \\]



Illustrative estimate

&#x20;We should plan for approximately 5,800 requests per second under these assumptions.



Ravi Sir:

But remember, average traffic is not enough! A movie release, cricket final or special sale can create sudden traffic spikes. Therefore, we need to consider:

- Average requests per second.
- Peak requests per second.
- Concurrent users.
- Read/write ratio.
- Average payload size.
- Storage growth.
- Network bandwidth.
- Recovery requirements.

# 6. Architecture trade-offs – The real architect's job

There is no universally perfect architecture.

Every design involves trade-offs.

| Decision                 | Option A         | Option B      |
| ------------------------ | ---------------- | ------------- |
| Application architecture | Monolith         | Microservices |
| Communication            | Synchronous      | Asynchronous  |
| Storage                  | SQL              | NoSQL         |
| Scaling                  | Vertical         | Horizontal    |
| Consistency              | Strong           | Eventual      |
| Caching                  | Cache-aside      | Write-through |
| Deployment               | Virtual machines | Containers    |

Ravi Sir: Suppose a student says: "Sir, microservices are always better than monolithic architecture." What would you tell that student?

Student: Sir, microservices introduce distributed-system complexity. For a small application, a modular monolith may be better.

Ravi Sir: Excellent! An architect is not someone who knows the maximum number of technologies. An architect is someone who knows when to use them and why.

# 7. Architecture Decision Records (ADR)

In real projects, architecture decisions should not remain only in developers' minds. We document important decisions.

Example:

ADR-001: Choose Redis for URL caching

Accepted decision

Context: URL redirection is a read-heavy workload.

Decision: Use Redis as a cache for frequently accessed short-code mappings.

Alternatives considered:

- Database-only lookup.
- In-memory application cache.
- Redis distributed cache.

Reason: Redis supports shared caching across multiple application instances.

Consequences:

- Lower database read load.
- Additional infrastructure to operate.
- Cache invalidation and expiry must be handled.

Other examples of ADRs:

- Why did we select WebSocket for messaging?
- Why did we choose SQL transactions for seat reservations?
- Why did we introduce a message broker?
- Why did we separate media storage from message storage?

# 8. Transflower classroom assignment

Practical system design challenge Design an Online Learning Platform for Transflower. Imagine 10,000 students using the platform.

### Business features

1. Student registration and login.
2. Course catalogue.
3. Video lessons.
4. Live classroom sessions.
5. Assignments.
6. Online assessments.
7. Mentor feedback.
8. Student progress tracking.
9. Certificates.
10. Employer dashboard.

### Your architecture must answer these questions

- How will students access video content?
- How will thousands of students attempt assessments simultaneously?
- How will mentors monitor student progress?
- How will assessment results be generated?
- How will the system handle temporary network failures?
- How will student data be protected?
- How will we scale when the student population grows?

### Expected deliverables

```
1. Business Requirement Document
2. Functional Requirements
3. Non-Functional Requirements
4. Capacity Estimation
5. System Context Diagram
6. Component Architecture
7. Database Architecture
8. Communication Architecture
9. Deployment Architecture
10. Architecture Decision Records
```

### Assessment rubric

| Criteria                    | Marks |
| --------------------------- | ----- |
| Requirement analysis        | 10    |
| Capacity estimation         | 10    |
| Component architecture      | 20    |
| Database design             | 15    |
| Communication design        | 15    |
| Scalability and reliability | 15    |
| Security and deployment     | 10    |
| Architecture explanation    | 5     |
| Total                       | 100   |

# 9. Final mentor discussion

Ravi Sir: Students, let's summarize our three case studies.

- URL Shortener taught us how to design a read-heavy system.
- WhatsApp taught us how to design a real-time distributed system.
- BookMyShow taught us how to handle concurrency and transactional consistency.

What is the most important lesson?

Student: Sir, different business problems require different architectural decisions.

Ravi Sir: Absolutely! Never design a system just because you know React, .NET, Java, Python, Kafka or Kubernetes. Understand the business problem first. Then design the system. Then select the technology.

```
             ENGINEERING THINKING

                 Problem
                    |
                    v
              Requirements
                    |
                    v
             Architecture
                    |
                    v
                Design
                    |
                    v
              Technology
                    |
                    v
            Implementation
                    |
                    v
                Testing
                    |
                    v
               Production
                    |
                    v
               Evolution
```

At Transflower, our objective is to develop engineers who can think beyond code.

A good developer can implement a feature.

A good designer can organize components.

A good architect can understand the entire system, anticipate failure and make informed trade-offs.

## 10. Continue the System Design journey

The next logical classroom is Distributed Systems Fundamentals:

- CAP theorem.
- Strong consistency vs eventual consistency.
- ACID vs BASE.
- Database replication and sharding.
- Load balancing.
- Message queues and event streaming.
- Saga pattern.
- Distributed transactions.
