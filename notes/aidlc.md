# From SDLC to AIDLC: Building an AI-Powered TFLCoMentor



Architecture + AI Engineering + Real-World Project


Theme: Traditional Software Engineering is evolving into AI-Driven Software Engineering.

Today's classroom objective: Understand why building an AI-powered product requires a different engineering lifecycle and how we can apply that lifecycle to build TFLCoMentor as an AI-powered Talent Development and Discovery Platform.

## 1. Let's begin with a classroom conversation

👨‍🏫 Mentor (Ravi Sir):Om, suppose I ask you to develop a Student Assessment System. What will you do first?

👨‍🎓 Student: Sir, first I will understand the requirements.

👨‍🏫 Mentor: Correct! What happens next?

👨‍🎓 Student: I will design the database, create the application architecture, develop APIs, build the frontend, test the application and deploy it.

👨‍🏫 Mentor: Excellent! This is the traditional Software Development Life Cycle (SDLC). Now let me change the problem. Suppose I want to develop an AI-powered Student Mentor System.

A student asks: "Sir, I am good at C#, but weak in Data Structures. What should I learn next to become an industry-ready developer?"

Can you solve this problem using only traditional CRUD operations?

👨‍🎓 Student: No, sir. We need to analyze student performance, understand skill gaps and recommend learning activities.

👨‍🏫 Mentor: Exactly! Now we have entered a different engineering world. We are not simply developing software that executes predefined business rules. We are developing a system that uses data, models, context, evaluation and continuous feedback to generate intelligent outcomes.

That brings us to AIDLC — AI Development Life Cycle.

## 2. SDLC vs AIDLC: Understand the difference

Think about two different engineering projects.

- Project A: Build a banking transaction application.
- Project B: Build an AI-powered financial advisory assistant.

Both require software engineering, architecture, APIs, databases and testing.

But Project B introduces additional engineering responsibilities: data preparation, model selection, model evaluation, inference monitoring and continuous optimization.

### Traditional SDLC

```
Requirement Gathering
        |
        v
System Analysis
        |
        v
Architecture & Design
        |
        v
Implementation
        |
        v
Testing & Quality Assurance
        |
        v
Deployment
        |
        v
Maintenance
```

### AI Development Life Cycle (AIDLC)

```
Problem Framing
        |
        v
Data Acquisition
        |
        v
Data Preparation
        |
        v
Model Building
        |
        v
Training & Evaluation
        |
        v
Deployment
        |
        v
Performance Monitoring
        |
        v
Continuous Optimization
        |
        +-----------> Feedback Loop
```

Mentor's observation:

In traditional software, we primarily engineer the logic. In AI products, we engineer the logic, data, models and feedback mechanisms together. 

One important clarification: AIDLC does not eliminate software architecture or system design. Those remain essential engineering activities. The lifecycle adds AI-specific activities that traditional SDLC does not explicitly emphasize.

## 3. Six stages of SDLC vs AIDLC

Let's understand each stage through a practical engineering perspective.

### Stage 1: Starting Point — Requirements vs Problem Framing

SDLC: Requirements Gathering

We ask:

- What should the application do?
- Who are the users?
- What business operations must be supported?
- What are the functional and non-functional requirements?

AIDLC: Problem Framing and Use Case Identification

We ask:

- What problem are we trying to solve?
- Does the problem actually require AI?
- What data is available?
- What should the AI predict, classify, recommend or generate?
- How will we measure success?

👨‍🏫 Mentor: Suppose a student scores 40% in SQL. Do we need an LLM to identify that the student scored 40%?

👨‍🎓 Student: No, sir. A simple database query can identify that.

👨‍🏫 Mentor: Then why introduce AI unnecessarily? A SQL query can identify weak performance. A recommendation engine can suggest exercises. An LLM can explain SQL joins using examples suited to the learner.

Remember: AI is not the solution to every software problem.

### Stage 2: Design vs Data

SDLC: System Architecture and Design

We design:

- Application architecture.
- Database schema.
- Components and modules.
- API contracts.
- Security architecture.
- Integration mechanisms.

AIDLC: Data Acquisition and Preparation

We prepare:

- Assessment results.
- Coding exercise submissions.
- Project evaluation records.
- Mentor feedback.
- Learning activity logs.
- Skill taxonomy.
- Learning resources.

👨‍🏫 Mentor: Imagine you are preparing food in a restaurant. You have an excellent chef, modern kitchen and expensive equipment. But what happens if your raw ingredients are spoiled?

👨‍🎓 Student: The food quality will be poor, sir.

👨‍🏫 Mentor: Exactly! An AI model is like a chef. Data is like the ingredients. Poor-quality data can produce poor-quality predictions, even when sophisticated algorithms are used. For TFLCoMentor, we need reliable learning evidence before attempting sophisticated AI.

### Stage 3: Build — Implementation vs Model Building

SDLC: Implementation

Developers write:

- Backend APIs.
- Frontend components.
- Database operations.
- Authentication.
- Business services.

AIDLC: Model Building

AI engineers work on:

- Algorithm selection.
- Model architecture.
- Training pipelines.
- Feature engineering.
- Model configuration.

Consider the following TFLCoMentor example.

Input:

```
Assessment Score       : 55
Coding Problems Solved : 20
Project Completion     : 40%
Mentor Feedback        : Needs improvement
```

Possible implementation approaches:

| Approach          | Purpose                                                |
| ----------------- | ------------------------------------------------------ |
| Rule-based engine | Identify predefined skill gaps                         |
| Scikit-learn      | Predict learning or readiness patterns                 |
| LLM               | Explain concepts and generate learning guidance        |
| RAG               | Retrieve relevant course material and mentor resources |

Mentor's lesson: Start with simple deterministic logic. Introduce machine learning or LLMs only when they provide measurable value.

### Stage 4: Test — Deterministic vs Probabilistic Evaluation

This is one of the biggest differences. 

Traditional software testing

```
Input:  10 + 20
Output: 30

Expected = Actual
PASS
```

For a deterministic function, the same input under the same conditions should produce the same output.

AI testing

```
Input:
Student needs help understanding recursion.

Possible Response 1:
Explain recursion using factorial.

Possible Response 2:
Explain recursion using a family tree.

Possible Response 3:
Explain recursion using nested function calls.
```

Different responses can all be correct.

We need to evaluate:

- Factual correctness.
- Relevance.
- Grounding in source material.
- Completeness.
- Safety.
- Consistency.
- Latency and cost.

👨‍🎓 Student: Sir, then how do we write assertions for AI responses?

👨‍🏫 Mentor: Good question!  We don't always assert exact text equality. We evaluate whether the generated answer satisfies defined quality criteria.

For example:

```
assert response is not Noneassert len(response) > 20assert is_relevant(response, question)assert is_grounded(response, retrieved_context)assert not contains_unauthorized_data(response)
```

These are illustrative checks. In production, relevance and grounding checks also require carefully designed evaluators and test datasets.

### Stage 5: Ship — Deployment vs Deployment and Monitoring

SDLC: Deployment

```
Build
  |
  v
Test
  |
  v
Release
  |
  v
Production
```

AIDLC: Deployment and Performance Monitoring

AI deployment is not the end of engineering.

We continuously observe:

- Model response quality.
- Prediction accuracy.
- Data drift.
- Model drift.
- Hallucination patterns.
- Response latency.
- Token consumption.
- API failures.
- User feedback.

Imagine TFLCoMentor recommends SQL exercises based on historical student performance. After six months, the curriculum changes. New technologies are introduced. The old recommendations may no longer be relevant. We need to monitor and improve the system.

### Stage 6: Maintain — Software Maintenance vs Continuous AI Improvement

SDLC

- Fix software bugs.
- Add new features.
- Improve performance.
- Upgrade dependencies.
- Maintain infrastructure.

AIDLC

- Improve data quality.
- Update knowledge bases.
- Re-evaluate models.
- Tune prompts.
- Improve retrieval.
- Fine-tune models when justified.
- Monitor changing data patterns.

Important distinction: A deployed model does not necessarily need continuous retraining. Some AI products need knowledge-base updates, prompt improvements or evaluation changes instead. The improvement strategy depends on the product.

## 4. Now connect AIDLC with TFLCoMentor

👨‍🏫 Mentor: Let's stop discussing hypothetical applications. We have a real engineering challenge. We want to build TFLCoMentor. What is the problem we are trying to solve?

👨‍🎓 Student: Sir, students complete courses and receive certificates, but companies want proof of actual development skills.

👨‍🏫 Mentor: Exactly! Today, many education systems measure:

```
Marks
Degrees
Certificates
Course Completion
```

But companies want to understand:

```
Can the candidate solve problems?

Can the candidate write quality code?

Can the candidate design APIs?

Can the candidate build projects?

Can the candidate learn independently?
```

This is the gap TFLCoMentor should address.

### Our product vision

# TFLCoMentor

## AI-Powered Talent Development and Discovery Platform

 
Students : Build skills and prove capability.
 

Mentors : Guide and accelerate learning.
 

Employers : Discover evidence-backed talent.

Think of TFLCoMentor as GitHub for student capabilities. GitHub showcases developer capability through code repositories. TFLCoMentor should showcase student capability through learning evidence.

### Student Talent Profile

```
Student: Rahul Patil
Technology Track: .NET Developer

SKILL GRAPH
--------------------------------
C#                  82%
ASP.NET Core        76%
SQL                 69%
Data Structures     64%

PROJECTS
--------------------------------
Online Store API
Student Assessment System
RabbitMQ Microservice

CODING ACTIVITY
--------------------------------
Problems Solved: 128

MENTOR FEEDBACK
--------------------------------
Strength:
Backend development

Improvement:
System design
```

The percentages above are illustrative. Actual skill scores must be supported by assessment evidence, project evaluation and mentor observations.

## 5. Five AI intelligence services for TFLCoMentor

Now let's think like system architects.

Instead of creating one giant AI application, identify business capabilities and separate them into logical services.

```
                 TFLCoMentor
                      |
                API Gateway
                      |
          Application Backend
                      |
             AI Intelligence
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
   Skill          Learning       Industry
 Intelligence   Recommendation  Readiness
   Service         Engine         Service
       |              |              |
       +--------------+--------------+
                      |
              Mentor Assistance
                      |
                      v
              Talent Discovery
                      |
                      v
                Data Platform
```

### Service 1: Student Skill Intelligence

Question: What skills does this student actually possess?

Inputs:

- Assessment results.
- Coding exercises.
- Project work.
- Mentor feedback.

Output:

```
Skill Profile

C#                  82%
ASP.NET Core        75%
SQL                 69%
Data Structures     64%
```

This service powers the Student Skill Graph and Talent Profile.

### Service 2: Learning Recommendation Engine

Question: What should this student learn next?

Example:

```
Weak Areas:
- Recursion
- SQL Joins

Recommended Activities:
1. Solve 5 recursion problems.
2. Build a small recursive program.
3. Practice SQL JOIN queries.
4. Complete a database mini-project.
```

Start with a rule-based recommendation engine. Later, introduce ML-based recommendations if the data justifies it.

### Service 3: Industry Readiness Scoring

Question: Is this student prepared for an interview?

The supplied concept proposes:

```
Industry Readiness =

(Assessment Score × 0.3)
+ (Project Completion × 0.4)
+ (Coding Activity × 0.2)
+ (Mentor Feedback × 0.1)
```

Illustrative output:

```
Score: 84 / 100
Status: Interview Ready
```

The weights are a proposed starting hypothesis, not a validated measure of employability. Readiness should be evidence-based, transparent and reviewed by mentors. It should not automatically determine a student's career opportunities.

### Service 4: Mentor Assistance Engine

Question: Which students need my attention today?

```
MENTOR DASHBOARD
--------------------------------
Students needing help:       4
Ready for advanced projects: 6
At-risk learners:            2

Priority:
Rahul  - Weak in algorithms
Sneha  - Missed assessments
Amit   - Ready for advanced work
```

This helps mentors spend less time manually analyzing records and more time mentoring students.

### Service 5: Talent Discovery Engine

Question: Can companies discover candidates using real skills?

Example recruiter query:

```
Technology: Java
Framework: Spring Boot
Skill: Microservices
Readiness Score: > 80
Projects: > 2
```

Illustrative output:

```
12 matching students found
```

Companies should see verified project evidence and skills, with appropriate student consent and access controls, rather than relying on an unexplained AI score.

## 6. The data engineering foundation

👨‍🏫 Mentor: Before developing these five services, tell me something. Where will the AI system get its knowledge?

👨‍🎓 Student: From the database, sir.

👨‍🏫 Mentor: Correct. But simply connecting an LLM to MySQL will not automatically create learning intelligence. We need to organize our learning data.

```
             Learning Data Platform
                      |
       +--------------+--------------+
       |              |              |
       v              v              v
  Assessment       Project        Coding
    Results      Submissions     Exercises
       |              |              |
       +--------------+--------------+
                      |
                      v
                Mentor Feedback
                      |
                      v
               Student Activity
                      |
                      v
              Data Processing
                      |
                      v
             Feature Engineering
                      |
             +--------+--------+
             |                 |
             v                 v
        Analytics          AI / ML
             |                 |
             +--------+--------+
                      |
                      v
              Learning Insights
```

The data platform may contain:

| Data source       | Example                          |
| ----------------- | -------------------------------- |
| Assessment        | Python score, SQL score          |
| Coding            | Problems attempted, test results |
| Projects          | Completion, complexity, review   |
| Mentor feedback   | Strengths, improvement areas     |
| Learning activity | Practice frequency, progress     |
| Skill taxonomy    | C#, SQL, APIs, algorithms        |

Architecture principle: Separate operational application data, analytical data and AI knowledge resources. They have different access patterns and purposes.

## 7. Event-driven learning intelligence

This is where software engineering and AI engineering come together. Suppose a student completes an assessment. What should happen next?

```
Student
   |
   v
Submits Assessment
   |
   v
Assessment API
   |
   v
Assessment Result Saved
   |
   v
Event Published
   |
   v
AssessmentCompleted
   |
   +-------------------------+
   |                         |
   v                         v
Skill Intelligence     Learning Analytics
   |                         |
   v                         v
Update Skill Graph     Update Progress
   |                         |
   +------------+------------+
                |
                v
       Recommendation Engine
                |
                v
       Updated Learning Plan
                |
                v
          Student Dashboard
```

Example event:

```
{
  "eventType": "AssessmentCompleted",
  "studentId": 1024,
  "assessmentId": 501,
  "topic": "SQL",
  "score": 55,
  "completedAt": "2026-11-06T10:30:00"
}
```

Technologies we can explore:

- RabbitMQ or Kafka for messaging.
- ASP.NET Core for application services.
- Python for analytics and ML.
- MySQL for operational data.
- React for dashboards.

A useful distinction: the event-driven pipeline can update deterministic skill calculations immediately, while AI-generated recommendations can run asynchronously.

## 8. Technology stack: From architecture to implementation

We should not select technologies simply because they are popular. We should select them based on responsibilities.

| Layer               | Suggested technology     | Responsibility                |
| ------------------- | ------------------------ | ----------------------------- |
| Frontend            | React                    | Student and mentor dashboards |
| Application backend | ASP.NET Core             | Business APIs and security    |
| AI services         | Python + FastAPI         | ML and AI orchestration       |
| Database            | MySQL                    | Operational learning data     |
| Messaging           | RabbitMQ                 | Event-driven integration      |
| ML                  | Scikit-learn             | Prediction and classification |
| LLM                 | OpenAI / Local LLM       | Explanation and generation    |
| Knowledge retrieval | FAISS / Chroma           | RAG-based retrieval           |
| Monitoring          | OpenTelemetry + Langfuse | Observability and evaluation  |
| Deployment          | Docker                   | Packaging and deployment      |

### One important architecture decision

For our existing TFLCoMentor engineering team, I would recommend starting with a modular monolith, not immediately splitting everything into microservices.

```
             React Frontend
                    |
                    v
             ASP.NET Core API
                    |
       +------------+------------+
       |            |            |
       v            v            v
   Membership   Assessment   Skill Taxonomy
       |            |            |
       +------------+------------+
                    |
                    v
             AI Orchestration
                    |
        +-----------+-----------+
        |           |           |
        v           v           v
      RAG        AI Tools    Evaluation
        |           |           |
        +-----------+-----------+
                    |
                    v
             Python AI Services
                    |
                    v
             MySQL / Knowledge
```

Keep module boundaries clean. Extract independent services when scaling, deployment or team ownership actually requires it.

## 9. AI features become student engineering projects

Now comes the most interesting part of our classroom. Instead of assigning students unrelated programming assignments, we can assign them engineering projects that contribute to TFLCoMentor. Each project teaches Software Engineering + Data Engineering + AI/ML + System Design.

| Project                        | Student engineering outcome      |
| ------------------------------ | -------------------------------- |
| Student Skill Graph Engine     | SQL analytics, APIs, aggregation |
| Learning Recommendation Engine | Rules, recommendation algorithms |
| Industry Readiness Predictor   | ML, feature engineering          |
| Learning Behavior Analyzer     | Event logging, analytics         |
| Mentor Insight Dashboard       | React, visualization, APIs       |
| Talent Portfolio Generator     | Profile and portfolio APIs       |
| Talent Discovery Engine        | Search, filtering, ranking       |

### Project 1: Student Skill Graph Engine

Foundation project

Problem statement

Develop an API that calculates student skill scores from assessment results, coding exercises, project evaluations and mentor feedback.

Input:

```
Assessment Score       = 80
Coding Exercise Score  = 70
Project Score          = 90
Mentor Feedback Score  = 85
```

One possible weighted calculation:

```
skill_score = ( assessment_score * 0.30 +    coding_score * 0.20 +    project_score * 0.40 +    mentor_score * 0.10)
```

Output:

```
{
  "studentId": 1024,
  "skill": "Backend Development",
  "score": 81.5
}
```

Students learn:

1. Database design.
2. SQL aggregation.
3. REST API development.
4. Data validation.
5. Business logic.
6. Unit and integration testing.

### Project 2: Learning Recommendation Engine

Problem statement

Develop an engine that recommends learning activities based on skill gaps.

```
if sql_score < 60:    recommendations.append(        "Practice SQL JOIN queries"    )if recursion_score < 60:    recommendations.append(        "Solve recursion problems"    )
```

This is a good example of introducing intelligence without requiring an LLM.

### Project 3: Industry Readiness Predictor

Problem statement

Develop a predictive model using historical, appropriately consented and validated learning data.

Students explore:

- Logistic regression.
- Decision trees.
- Feature engineering.
- Training and test datasets.
- Precision and recall.
- Model evaluation.

Remember: historical placement data may contain bias. A model should be validated before anyone uses its predictions to make career decisions.

### Project 4: Mentor Insight Dashboard

Problem statement

Build a dashboard that identifies students requiring attention.

```
Mentor Dashboard
-----------------------------

Students needing attention: 4

Weak in algorithms:          2
Missed assessments:          1
Project completion delayed:  1

Advanced project ready:      5
```

This is a practical full-stack engineering assignment.

### Project 5: Talent Discovery Engine

Problem statement

Build a recruiter-facing search API.

```
GET /api/talent/search?technology=Java&skill=Microservices&minimumScore=80
```

The application should return candidates whose skills are backed by actual project and assessment evidence, subject to employer authorization and student consent.

## 10. The complete TFLCoMentor AI Development Lifecycle

Let's combine everything into one engineering picture.

```
              TFLCoMentor
                   |
                   v
          1. PROBLEM FRAMING
                   |
         Identify learning gaps
         Identify stakeholders
         Define success metrics
                   |
                   v
          2. DATA ENGINEERING
                   |
         Collect student evidence
         Clean and validate data
         Define skill taxonomy
                   |
                   v
          3. SYSTEM + MODEL DESIGN
                   |
         Application architecture
         Database and API contracts
         Select rules / ML / LLM
                   |
                   v
          4. IMPLEMENTATION
                   |
         Backend and frontend
         AI orchestration
         RAG and ML components
                   |
                   v
          5. EVALUATION
                   |
         Software testing
         Model evaluation
         Grounding and safety
         Human mentor review
                   |
                   v
          6. DEPLOYMENT
                   |
         Docker
         Production APIs
         Monitoring
                   |
                   v
          7. CONTINUOUS IMPROVEMENT
                   |
         Feedback collection
         Data quality monitoring
         Prompt/model optimization
                   |
                   +---------> Feedback Loop
```

Notice something important. AIDLC does not replace SDLC. AIDLC extends traditional Software Engineering with data, model and AI evaluation responsibilities.

## 11. Five production-readiness questions

Before deploying an AI-powered TFLCoMentor feature, ask:

| Gate                 | Engineering question                                      |
| -------------------- | --------------------------------------------------------- |
| Data readiness       | Is the student data accurate and authorized for this use? |
| Model readiness      | Is the selected model appropriate for the problem?        |
| Evaluation readiness | Have we tested correctness, bias and reliability?         |
| Security readiness   | Can users access only permitted student information?      |
| Operations readiness | Can we monitor, troubleshoot and improve the system?      |

For example, an AI Mentor Assistant must never expose Rahul's private assessment records to another student merely because that student asks a clever question.

AI-generated guidance must respect the same authorization boundaries as ordinary application APIs.

## 12. Classroom activity: Think like an AI engineer

👨‍🏫 Mentor: Now I have an assignment for you. We are going to develop an AI-powered learning recommendation feature. Suppose Rahul has the following performance:

```
C#                  85
ASP.NET Core        78
SQL                 45
Data Structures     52
Project Completion  60
```

Your task is to design a recommendation system.

Question 1: Which skills need improvement?

Question 2: Can we identify these weak skills using SQL queries instead of an LLM?

Question 3: How would you recommend the next five learning activities?

Question 4: Which data should we store to measure improvement over time?

Question 5: How will you test whether your recommendations are useful?

Question 6: How will a mentor approve, modify or reject AI recommendations?

Expected architecture:

```
Student Assessment
        |
        v
Skill Score Calculator
        |
        v
Skill Gap Analyzer
        |
        v
Recommendation Engine
        |
        v
AI Mentor Assistant
        |
        v
Mentor Review
        |
        v
Student Learning Plan
        |
        v
Next Assessment
        |
        +----------> Feedback Loop
```

The objective is not to build a chatbot. The objective is to build an intelligent learning system.

## 13. Mentor's concluding thoughts

👨‍🏫 Ravi Sir: Students, remember one thing. Twenty years ago, software developers primarily wrote programs. Then we started building distributed systems, cloud applications and microservices. Today, we are building systems that can understand context, use tools, analyze data and assist human decision-making. But technology evolution does not eliminate engineering fundamentals. In fact, it makes them more important. A developer who understands only prompts cannot build reliable AI products.

A developer who understands:

- Programming fundamentals.
- Data structures and algorithms.
- Database engineering.
- System design.
- API development.
- Software testing.
- Data engineering.
- AI and ML fundamentals.

...can become an AI engineer.

My message to Transflower students:

```
Don't just learn AI tools.

Learn how AI systems are engineered.

Don't just build chatbots.

Build intelligent products.

Don't just complete assignments.

Build systems that solve real problems.
```

### The final vision

# TFLCoMentor

## From Assessment System to Global Talent Discovery Platform

1. Assessment Platform :    Measure learning performance.
2. Mentor-Driven Learning Platform :   Guide students through projects and feedback.
3. AI-Powered Learning Intelligence :    Identify skill gaps and recommend learning paths.
4. Talent Discovery Platform :    Help companies discover evidence-backed developer capabilities.

Students build the intelligence of the platform that helps evaluate and develop their own capabilities.

## Tap your potential!

One sentence to remember:

> SDLC teaches us how to build software. AIDLC teaches us how to build, evaluate and continuously improve intelligent software systems.
