# Build your muscle not just memory

The biggest mistake I see students make with **System Design** is not choosing the wrong resource. It is **collecting too many resources and designing too few systems.** Here is how I would explain it as a Transflower Mentor.

### Which System Design Resource Should You Follow?

A student once asked me: **“Sir, which System Design course should I follow?”** I asked him: **“How many System Design courses have you already saved?”** He smiled. You know the answer. 
- YouTube playlists.
- Udemy courses.
- System Design books.
- HLD roadmaps.
- LLD roadmaps.
- Microservices.
- Distributed Systems.
- Cloud architecture.
- Database design.
- Kafka.
- Redis.
And another 50 bookmarked videos. Then I asked him: **“How many systems have you designed?”** Silence. And that is the real problem.

### You don't have a resource problem.

**You have a practice problem.** System Design is not something you learn by watching somebody else draw boxes on a whiteboard. You learn it by **making architectural decisions yourself.** So, if you ask me: **“Which System Design resource should I follow?”** My answer is: **Follow one structured resource.** But spend most of your time **designing systems.** Here is the roadmap I recommend.

### 1️⃣ Start with the building blocks
Don't start with: **“Design Netflix.”**
Start with: **“How does a web request travel through a system?”**
Understand:
- → Client–Server architecture
- → HTTP / HTTPS
- → REST APIs
- → DNS
- → Reverse Proxy
- → Load Balancer
- → Caching
- → Databases
- → Message Queues
- → CDN
- → Horizontal vs Vertical Scaling

These are your architectural building blocks. Think of them like **bricks.** You cannot build a house until you understand the bricks.
 

### 2️⃣ Learn databases beyond CRUD
Many developers say: **SQL = tables**, **NoSQL = documents** That's not System Design. Understand:
- → Indexing
- → Transactions
- → ACID
- → Replication
- → Sharding
- → Partitioning
- → Read Replicas
- → Consistency
- → CAP theorem

Then ask:**Why would I choose this database for this workload?** That question is much more valuable than memorizing database definitions.

### 3️⃣ Understand caching
One day a developer comes to you and says: “Our API is slow.” You add more servers.
Still slow. You add another server. Still slow. Then you discover that every request is hitting the database. That's when you learn an important System Design lesson: **Scaling the application layer doesn't solve every bottleneck.**

Learn:
- → Cache-aside
- → Write-through
- → Write-back
- → TTL
- → Eviction
- → Cache invalidation
- → Cache consistency
- → Distributed caching

And remember the famous engineering reality:
- **Caching is easy to introduce.**
- **Keeping cached data correct is the difficult part.**

### 4️⃣ Learn asynchronous communication
At small scale, this feels simple: **Request → API → Database → Response**

But imagine:
- 10 users.
- 100 users.
- 10,000 users.
- 10 million users.

Suddenly you don't want every operation waiting for everything else. That's where asynchronous architecture enters.Learn:
- → Message Queues
- → Pub/Sub
- → Kafka
- → Producers & Consumers
- → Retry
- → Dead Letter Queues
- → Idempotency
- → Event-driven architecture

Now you start thinking beyond: **“Does my API work?”** You start asking: **“What happens when part of my system is temporarily unavailable?”** That is System Design thinking.

### 5️⃣ Learn scalability

This is one of my favorite questions to ask students: **“Your application works perfectly with 100 users. What happens when tomorrow you have 10 million?”** Don't immediately say: **“Add more servers.”** Ask: 
- Where is the bottleneck?
- Application?
- Database?
- Network?
- Storage?
- External API?
- Queue?
- Cache?

Then think about:
- → Traffic spikes
- → Read-heavy workloads
- → Write-heavy workloads
- → Horizontal scaling
- → High availability
- → Fault tolerance
- → Replication
- → Load distribution

System Design is largely the art of finding and removing **bottlenecks**.

### 6️⃣ Now design real systems
This is where learning becomes fun.
Start small.

- 🔹 URL Shortener
- 🔹 Rate Limiter
- 🔹 Notification System
- 🔹 Parking Lot
- 🔹 Food Delivery
- 🔹 Ride Sharing
- 🔹 Chat Application
- 🔹 Payment System

Then move toward larger systems:
- 🔹 YouTube
- 🔹 Instagram
- 🔹 Netflix

But don't copy somebody else's architecture. Take a blank sheet. Start with:
- **1. Requirements** : What exactly are we building?
- **2. APIs** : How will clients communicate with it?
- **3. Data** : What information needs to be stored?
- **4. Architecture** :What components do we need?
- **5. Scalability** : What happens when traffic becomes 10x?
- **6. Reliability** :What happens when a component fails?
- **7. Consistency** :Which data must be strongly consistent?
- **8. Availability**: Can the system continue operating when something fails?
- **9. Observability** :How will we know something is broken?
- **10. Trade-offs** :What are we gaining?
What are we sacrificing?
 
### And here is the most important part.
Don't spend three months watching System Design videos. Watch one concept. Understand it. Close the video. Take a problem. **Design it yourself.** Your first design will probably be bad. Good. That's exactly what you need. Then review it. Find the bottleneck. Redesign it. Add caching. Add a queue. Introduce replication. Think about failure. Think about 10x traffic. Then redesign it again. That's how architectural thinking develops.

### A small challenge for my students
Take an **Insurance Management System.** Don't just build: Customer → Controller → Service → Repository → Database. That is application architecture. Now think like a System Designer. 
- What happens when: **10,000 customers submit claims simultaneously?** 
- What happens when: **the Claims API is unavailable?**
- What happens when: **the payment service times out?**
- What happens when: **the database becomes slow?**
- What happens when: **the same claim request arrives twice?**
- What happens when: **a customer uploads 100 documents?**
- What happens when: **claim processing takes 30 minutes?**

Now introduce: 
- → **API Gateway**
- → **Load Balancer**
- → **Cache**
- → **Database**
- → **Message Queue**
- → **Background Workers**
- → **Object Storage**
- → **Notification Service**
- → **Monitoring**
- → **Retry / DLQ**
- → **Idempotency**

Now you're not just learning System Design. **You're practicing it.**

### Remember this.
A developer who has watched **100 System Design videos** may still struggle with a blank whiteboard. A developer who has designed **20 systems**, failed, reviewed, redesigned, and understood the trade-offs... will start thinking like an architect. So don't ask only: **“Which System Design resource should I follow?”** Ask yourself: **“Which system am I going to design today?”** Because... **Watching someone design a system is knowledge.** **Designing one yourself is skill.** And at Transflower, our goal is not to create developers who can memorize architectures. Our goal is to create engineers who can **reason about systems, make trade-offs, solve problems, and explain why they made a particular design decision.**

🌱 **Transflower Mentor**
**Mentor takeaway:** Don't build a *System Design library* in your browser. Build a **System Design muscle** in your brain.