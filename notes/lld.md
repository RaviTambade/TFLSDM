# Low-Level Design (LLD) – From Writing Classes to Thinking Like a Software Engineer

SOFTWARE DESIGN · OBJECT-ORIENTED DESIGN · SOLID · DESIGN PATTERNS · UML

Good morning, everyone! Today, we are moving one level deeper. We are going to understand how to design the internal structure of an application. Let me ask you a question. Suppose Transflower wants to develop a Student Assessment Management System.

The application should support:

- Student registration.
- Mentor management.
- Skill assessment.
- Question bank.
- Evaluation.
- Performance reports.
- Placement eligibility.

You are asked to develop this system.What will you do first?

Student: Sir, I will create classes like Student, Mentor, Assessment and Question.

Mentor:Good! But let me ask another question.

- Who will evaluate the student's answers?
- Who will calculate marks?
- Who will decide whether an assessment is completed?
- Who will generate the result?
- Who will store the result?
- Who will notify the student?
- Should one class do everything?

Student:No, Sir. We should divide responsibilities.

Mentor: Exactly! That is where Low-Level Design begins. HLD tells us what major components our system needs. LLD tells us how those components collaborate internally.

## 1. HLD vs LLD – Understanding the Difference

Imagine you want to construct a hospital. An architect prepares the overall building plan.

- Emergency department.
- Operation theatre.
- Patient rooms.
- Pharmacy.
- Reception.
- Parking.

This is similar to High-Level Design. But who decides:

- How rooms are connected?
- Where electrical wiring goes?
- How elevators operate?
- How doors open?
- How equipment is installed?

That requires detailed design. This is similar to Low-Level Design.
 

### HLD – High-Level Design

Focuses on the overall system architecture.

```
        TRANSFLOWER ASSESSMENT SYSTEM

                  |
       +----------+----------+
       |          |          |
    Student    Assessment  Reporting
    Module      Module      Module
       |          |          |
       +----------+----------+
                  |
               Database
```

### LLD – Low-Level Design

Focuses on internal objects, classes, interfaces and their relationships.

```
          ASSESSMENT MODULE

        +------------------+
        |   Assessment     |
        +------------------+
        | id               |
        | title            |
        | duration         |
        +------------------+
        | start()          |
        | submit()         |
        +--------+---------+
                 |
                 |
        +--------v---------+
        |    Question      |
        +------------------+
        | id               |
        | questionText     |
        | marks            |
        +------------------+
```

| HLD                         | LLD                         |
| --------------------------- | --------------------------- |
| System architecture         | Object architecture         |
| Services and modules        | Classes and interfaces      |
| API boundaries              | Method contracts            |
| Database and infrastructure | Object relationships        |
| Deployment view             | Class and sequence diagrams |
| Technology decisions        | Implementation decisions    |

Mentor's takeaway:

HLD may get you started. LLD determines whether your code can evolve without becoming difficult to maintain.

## 2. OOP Fundamentals – The Foundation of LLD

Before designing complex systems, we must understand Object-Oriented Programming. Let's take a real-world example. A student is an object. A student has:

- Identity.
- Name.
- Email.
- Course.

A student can:

- Register.
- Enroll.
- Attempt assessment.
- View results.

This is object-oriented thinking.

### The four pillars of OOP

1. Encapsulation

Keep data and related operations together. Protect internal state through controlled methods.

Example: A BankAccount controls deposits and withdrawals rather than exposing its balance for arbitrary modification.


2. Abstraction

Expose essential operations while hiding implementation details.

Example: You drive a car using a steering wheel without needing to understand the engine's internal operation.


3. Inheritance

A class derives behavior and properties from another class.

Example: Employee and Mentor can share common properties from a Person class, where the relationship genuinely represents an is-a relationship.


4. Polymorphism

Different objects respond to the same operation in different ways.

Example: Different notification providers implement `send()` differently.

### Classroom coding example – Polymorphism

```
class Notification:    def send(self, message):        raise NotImplementedErrorclass EmailNotification(Notification):    def send(self, message):        print("Sending Email:", message)class SMSNotification(Notification):    def send(self, message):        print("Sending SMS:", message)class WhatsAppNotification(Notification):    def send(self, message):        print("Sending WhatsApp:", message)def notify(provider, message):    provider.send(message)notify(EmailNotification(), "Assessment completed")notify(SMSNotification(), "Assessment completed")notify(WhatsAppNotification(), "Assessment completed")
```

 Mentor:Students, observe carefully. The `notify()` function does not need to know how an email is sent, how SMS works or how WhatsApp works. It only expects an object that supports the `send()` operation. This is polymorphism in action.

Engineering principle: Depend on contracts, not implementation details.

## 3. SOLID Principles – Five Rules for Better Software Design

Now let's move from OOP to professional software design. SOLID is a collection of five design principles that help us develop maintainable and extensible object-oriented systems.
 

### S – Single Responsibility Principle

One class should have one cohesive responsibility, or one reason to change.

Bad design:

```
class Student:    def register(self):        pass    def save_to_database(self):        pass    def send_email(self):        pass    def generate_report(self):        pass
```

Why is this problematic? The Student class has too many unrelated responsibilities.

Better design:

```
Student
   |
   +---- StudentRepository
   |
   +---- StudentRegistrationService
   |
   +---- NotificationService
   |
   +---- StudentReportService
```

### O – Open/Closed Principle

Software components should be open for extension but closed for modification.

Example: You initially support email notifications. Later, you want SMS and WhatsApp. Instead of repeatedly changing a large notification class, introduce a common contract and add new implementations.

```
class NotificationProvider:    def send(self, message):        raise NotImplementedErrorclass EmailProvider(NotificationProvider):    def send(self, message):        print("Email:", message)class SMSProvider(NotificationProvider):    def send(self, message):        print("SMS:", message)
```

### L – Liskov Substitution Principle

Objects of a subtype should be usable wherever their base type is expected without breaking the expected behavior. For example, if a system expects every payment provider to process a supported payment, a derived provider should not unexpectedly reject valid operations because it violates the base contract.

### I – Interface Segregation Principle

Clients should not be forced to depend on methods they do not need. Instead of one giant interface:

```
IStudentOperations
   |
   +-- register()
   +-- evaluate()
   +-- generate_invoice()
   +-- send_notification()
   +-- manage_courses()
```

Prefer smaller, cohesive contracts:

```
IStudentRegistration
IAssessmentEvaluator
IInvoiceGenerator
INotificationSender
ICourseManager
```

### D – Dependency Inversion Principle

High-level business logic should not depend directly on low-level implementation details. Both should depend on abstractions.

Bad:

```
class AssessmentService:    def __init__(self):        self.repository = MySQLRepository()
```

Better:

```
class AssessmentService:    def __init__(self, repository):        self.repository = repository
```

The concrete repository is supplied from outside. This makes testing and changing infrastructure easier.

### SOLID summary

| Principle | Engineering question                                           |
| --------- | -------------------------------------------------------------- |
| SRP       | Does this class have too many responsibilities?                |
| OCP       | Can I add behavior without repeatedly modifying existing code? |
| LSP       | Can a subtype safely replace its base type?                    |
| ISP       | Are clients forced to depend on unnecessary operations?        |
| DIP       | Does business logic depend on abstractions?                    |

Mentor's message: SOLID is not a collection of rules to apply blindly. It is a way to reason about change, responsibility and dependencies.

## 4. Design Patterns – Reusable Solutions to Common Problems

Mentor: Suppose you are developing an application. You encounter a problem that another developer has already solved in a different application. Would you reinvent the solution every time?

 Student: No, Sir. We can reuse an established design approach.

 Mentor: Correct! That is the motivation behind Design Patterns.

A design pattern is a reusable approach to a commonly occurring software design problem. It is not a ready-made code library.

There are three major categories.

### A. Creational Patterns

These deal with object creation.

Factory Pattern

Creates objects without exposing all their construction details.

Example: Select Email, SMS or WhatsApp notification provider.

Builder Pattern

Constructs complex objects step by step.

Example: Building an assessment with title, questions, duration, marks and instructions.


Singleton Pattern

Ensures a class has one controlled instance.

Example: A shared configuration object. Use carefully; global mutable singletons can make testing difficult.

### B. Structural Patterns

These deal with object composition and relationships.

- Adapter: Makes incompatible interfaces work together.
- Decorator: Adds behavior to an object without changing its original class.

Example – Decorator:

Suppose we have a basic notification service.

We want to add logging without modifying its original implementation.

```
class Notification:    def send(self, message):        print("Sending:", message)class LoggingDecorator:    def __init__(self, notification):        self.notification = notification    def send(self, message):        print("Logging notification...")        self.notification.send(message)service = LoggingDecorator(Notification())service.send("Assessment completed")
```

### C. Behavioral Patterns

These deal with communication and responsibilities between objects.

| Pattern  | Example                                            |
| -------- | -------------------------------------------------- |
| Strategy | Different assessment evaluation algorithms         |
| Observer | Notify students when results are published         |
| Command  | Submit, cancel or schedule an assessment operation |
| State    | Assessment lifecycle: Draft, Published, Completed  |

### Strategy Pattern – A practical example

Imagine students can be evaluated using different assessment strategies.

```
       AssessmentEvaluator
               |
        EvaluationStrategy
               |
       +-------+-------+
       |               |
   MCQStrategy     CodingStrategy
       |               |
    Evaluate         Evaluate
    objective        test cases
    answers          and logic
```

Python implementation:

```
class EvaluationStrategy:    def evaluate(self, submission):        raise NotImplementedErrorclass MCQStrategy(EvaluationStrategy):    def evaluate(self, submission):        return 8class CodingStrategy(EvaluationStrategy):    def evaluate(self, submission):        return 9class AssessmentEvaluator:    def __init__(self, strategy):        self.strategy = strategy    def evaluate(self, submission):        return self.strategy.evaluate(submission)evaluator = AssessmentEvaluator(MCQStrategy())print(evaluator.evaluate("student submission"))
```

Notice that the evaluator doesn't need to know the internal evaluation algorithm. We can change the strategy without rewriting the evaluator.

Mentor's lesson: Don't memorize 23 design patterns. Learn to recognize recurring design problems and select an appropriate solution.

## 5. UML Diagrams – Design Before Coding
 Mentor: Before constructing a building, what does an architect prepare?

 Student: Blueprints, Sir.
 Mentor: Exactly! Similarly, before implementing a complex software system, engineers use diagrams to communicate design decisions. UML stands for Unified Modeling Language. Let's understand four important diagrams.

### A. Class Diagram

Shows classes, attributes, operations and relationships.

Example:

```
+-----------------------+
|       Student         |
+-----------------------+
| - studentId: int      |
| - name: string        |
| - email: string       |
+-----------------------+
| + register()          |
| + enroll()            |
| + viewResult()        |
+-----------------------+
           |
           | enrolls
           |
           v
+-----------------------+
|        Course         |
+-----------------------+
| - courseId: int       |
| - title: string       |
+-----------------------+
| + getDetails()        |
+-----------------------+
```

### B. Sequence Diagram

Shows how objects interact over time.

Example: Student login.

```
 Student       Browser       Controller       Service       Database
    |             |               |               |              |
    | Login       |               |               |              |
    |------------>|               |               |              |
    |             | POST /login   |               |              |
    |             |-------------->|               |              |
    |             |               | authenticate  |              |
    |             |               |-------------->|              |
    |             |               |               | Query User   |
    |             |               |               |------------->|
    |             |               |               | User Record  |
    |             |               |               |<-------------|
    |             |               | Auth Result   |              |
    |             |               |<--------------|              |
    |             | Login Response|               |              |
    |             |<--------------|               |              |
    | Dashboard   |               |               |              |
    |<------------|               |               |              |
```

### C. Activity Diagram

Shows the workflow and decision-making process.

Example: Assessment submission.

```
        START
          |
          v
   Student Login
          |
          v
   Open Assessment
          |
          v
   Attempt Questions
          |
          v
   Submit Answers
          |
          v
    Validate Submission
          |
          v
    Is Valid?
       /    \
     Yes     No
      |       |
      v       v
   Evaluate  Show Error
      |
      v
   Generate Result
      |
      v
      END
```

### D. Object Diagram

Shows actual object instances at a particular point in time.

```
student1 : Student
------------------
studentId = 101
name = "Om"
course = "Python"


course1 : Course
----------------
courseId = 501
title = "Python Development"
```

### When should we use which diagram?

| Diagram  | Main question                                        |
| -------- | ---------------------------------------------------- |
| Class    | What objects and relationships exist?                |
| Sequence | How do objects communicate?                          |
| Activity | What is the workflow?                                |
| Object   | What does the object structure look like at runtime? |

Engineering takeaway: A diagram is not decoration. It is a communication tool that makes design decisions visible.

## 6. Real-World System Modeling – Design a Parking Lot

Now let's perform an actual LLD exercise.
 
Problem statement:

Design a Parking Lot Management System.

Requirements:

1. Parking lot has multiple floors.
2. Each floor contains multiple parking spaces.
3. Different types of vehicles can enter.
4. Parking spaces have different sizes.
5. System allocates an appropriate available space.
6. Entry and exit are recorded.
7. Parking fee is calculated.
8. Payment is processed.
 Mentor:

Students, should we immediately start writing Python classes?

Student: No, Sir. First, we should identify entities and responsibilities.

### Step 1: Identify Objects

```
ParkingLot
ParkingFloor
ParkingSpace
Vehicle
ParkingTicket
ParkingFeeCalculator
Payment
EntryGate
ExitGate
```

### Step 2: Identify Relationships

```
              ParkingLot
                   |
                   | contains
                   v
              ParkingFloor
                   |
                   | contains
                   v
              ParkingSpace
                   ^
                   |
                allocated
                   |
                 Vehicle
                   |
                   v
              ParkingTicket
                   |
                   v
           ParkingFeeCalculator
                   |
                   v
                Payment
```

### Step 3: Identify responsibilities

| Class                | Responsibility                                  |
| -------------------- | ----------------------------------------------- |
| ParkingLot           | Manage floors and overall capacity              |
| ParkingFloor         | Manage spaces on a floor                        |
| ParkingSpace         | Maintain availability and vehicle compatibility |
| Vehicle              | Represent vehicle details                       |
| ParkingTicket        | Record entry and exit information               |
| ParkingFeeCalculator | Calculate charges                               |
| Payment              | Record payment status                           |
| EntryGate            | Coordinate vehicle entry                        |
| ExitGate             | Coordinate vehicle exit                         |

### Step 4: Model extensibility

Suppose tomorrow the parking lot supports:

- Motorcycle
- Car
- Bus
- Electric vehicle

Should we write separate parking algorithms for every vehicle?

Not necessarily.

We can introduce abstractions.

```
from enum import Enumclass VehicleType(Enum):    MOTORCYCLE = "motorcycle"    CAR = "car"    BUS = "bus"class Vehicle:    def __init__(self, registration, vehicle_type):        self.registration = registration        self.vehicle_type = vehicle_typeclass ParkingSpace:    def __init__(self, space_id, supported_types):        self.space_id = space_id        self.supported_types = supported_types        self.is_available = True    def can_park(self, vehicle):        return (            self.is_available            and vehicle.vehicle_type in self.supported_types        )    def park(self, vehicle):        if not self.can_park(vehicle):            raise ValueError("Parking space unavailable")        self.is_available = False
```

This is a small starting model, not a complete parking system. A production design would also address concurrent allocation, reservations, ticket lifecycle, payment failures and transaction consistency.

Mentor's observation: LLD is the process of translating real-world requirements into objects, responsibilities and interactions.

## 7. Interface and Abstraction Design – The Difference Between Junior and Senior Thinking

Let's consider a real development scenario.

Your Transflower Assessment System initially supports MCQ evaluation.

Later, you want to introduce:

- Programming evaluation.
- SQL query evaluation.
- Practical lab evaluation.
- Video presentation evaluation.

What happens if all evaluation logic is written inside one large class?

```
class AssessmentEvaluator:    def evaluate(self, assessment_type, answer):        if assessment_type == "MCQ":            pass        elif assessment_type == "CODING":            pass        elif assessment_type == "SQL":            pass        elif assessment_type == "VIDEO":            pass
```

Every time a new assessment type is introduced, we modify this class.

This can become difficult to maintain.

Instead, use an abstraction.

```
class IEvaluator:    def evaluate(self, submission):        raise NotImplementedErrorclass MCQEvaluator(IEvaluator):    def evaluate(self, submission):        return "Evaluating MCQ"class CodingEvaluator(IEvaluator):    def evaluate(self, submission):        return "Evaluating Code"class VideoEvaluator(IEvaluator):    def evaluate(self, submission):        return "Evaluating Presentation"
```

Now the application can work with the common evaluator contract.

```
             IEvaluator
                  |
       +----------+----------+
       |          |          |
      MCQ       Coding      Video
    Evaluator  Evaluator   Evaluator
```

This gives us:

- Loose coupling.
- Interface-driven development.
- Replaceable implementations.
- Easier testing.
- Better extensibility.

Remember: An abstraction defines what a component should do. An implementation defines how it does it.

## 8. Dependency Injection and Inversion of Control

Now let's connect this to the previous MVC lesson. Suppose your AssessmentService directly creates its database repository.

```
class AssessmentService:    def __init__(self):        self.repository = MySQLAssessmentRepository()
```

What is the problem? The service is tightly coupled to MySQL. Tomorrow, if you want PostgreSQL or an in-memory repository for testing, you need to modify the service.

Better approach:

```
class AssessmentService:    def __init__(self, repository):        self.repository = repository    def get_assessment(self, assessment_id):        return self.repository.get_by_id(            assessment_id        )
```

Now provide the dependency from outside.

```
class InMemoryAssessmentRepository:    def get_by_id(self, assessment_id):        return {            "id": assessment_id,            "title": "Python Fundamentals"        }repository = InMemoryAssessmentRepository()service = AssessmentService(repository)print(service.get_assessment(101))
```

### Dependency Injection Architecture

```
         APPLICATION STARTUP
                 |
                 v
        +-------------------+
        | DEPENDENCY SETUP  |
        +-------------------+
                 |
                 v
        +-------------------+
        | AssessmentService |
        +-------------------+
                 |
                 | Depends on abstraction
                 v
        +-------------------+
        | Repository        |
        | Contract          |
        +-------------------+
                 ^
                 |
         +-------+-------+
         |               |
      MySQL           InMemory
    Repository       Repository
```

There are three commonly discussed injection styles:

| Type                  | Description                                 |
| --------------------- | ------------------------------------------- |
| Constructor Injection | Dependency supplied through constructor     |
| Setter Injection      | Dependency supplied through a setter        |
| Method Injection      | Dependency supplied to an individual method |

Constructor injection is generally a good default for required dependencies.

Mentor's lesson: A well-designed class should not be responsible for constructing every object it needs.

## 9. Test-Driven Development – Design for Testability

Mentor: You have designed your AssessmentService. How do you know it works correctly?

Student: We should test it, Sir.

Mentor: Correct! But should testing happen only after development? Not necessarily. We can follow Test-Driven Development (TDD).

TDD follows three important steps:

1. Red: Write a test that initially fails.
2. Green: Write the minimum code required to pass.
3. Refactor: Improve the implementation while keeping tests green.

### Example – Assessment Result Calculation

Requirement: A student receives marks for five questions. The system should calculate total marks.

Step 1: Write a test

```
def test_calculate_total_marks():    marks = [10, 15, 20, 5, 10]    result = calculate_total(marks)    assert result == 60
```

Step 2: Implement the functionality

```
def calculate_total(marks):    return sum(marks)
```

Step 3: Run the test

```
pytest -v
```

### Testing concepts to master

| Concept             | Purpose                                           |
| ------------------- | ------------------------------------------------- |
| Unit Testing        | Test individual units                             |
| Mocking             | Replace dependencies with controlled test doubles |
| Stubbing            | Supply predetermined responses                    |
| Integration Testing | Test collaboration between components             |
| Regression Testing  | Ensure existing functionality still works         |

Good LLD makes testing easier because dependencies and responsibilities are clearly separated.

## 10. Clean Code – Write Code Humans Can Understand

Mentor: Imagine you write 500 lines of code today. After six months, another developer has to maintain it. Will they understand your code?

Student: Only if the code is structured properly and readable.

Mentor: Exactly!

Remember three principles.

### DRY – Don't Repeat Yourself

Avoid duplicating the same business logic in multiple places.

### KISS – Keep It Simple

Prefer simple solutions over unnecessary complexity.

### YAGNI – You Aren't Gonna Need It

Don't build features and abstractions before they are actually needed.

### Common code smells

| Code smell     | Problem                                            |
| -------------- | -------------------------------------------------- |
| God Class      | One class does everything                          |
| Duplicate Code | Same logic repeated                                |
| Tight Coupling | Components depend heavily on implementations       |
| Long Method    | Too many operations in one method                  |
| Magic Numbers  | Unexplained hardcoded values                       |
| Feature Envy   | A class relies excessively on another class's data |

Example:

Bad:

```
def process(s, t, a):    # What does this mean?    pass
```

Better:

```
def process_student_assessment(    student,    assessment,    answers):    pass
```

Code is written once but read many times. Optimize for readability.

## 11. Build Portfolio-Ready LLD Projects

Now let's convert the roadmap into practical engineering work.

I recommend that every Transflower student complete at least three substantial LLD projects.

 
Project 1 – BookMyShow

Concepts:

- Movie and theatre modeling
- Seat allocation
- Booking lifecycle
- Payment integration
- Concurrent seat reservation

Patterns: Strategy, State, Factory.

 

Project 2 – Uber-like Ride Booking

Concepts:

- Driver and passenger
- Ride matching
- Fare calculation
- Ride lifecycle
- Location updates

Patterns: Strategy, Observer, State.

 

Project 3 – ATM Machine

Concepts:

- Account validation
- PIN authentication
- Withdrawal
- Balance checking
- Cash availability

Patterns: State, Strategy, Chain of Responsibility.


Project 4 – Elevator System

Concepts:

- Floor requests
- Direction management
- Scheduling
- Door operations
- Safety constraints

Patterns: State, Strategy, Observer.

For every project, students must submit:

1. Problem statement.
2. Functional and non-functional requirements.
3. Use-case diagram.
4. Class diagram.
5. Sequence diagram.
6. Object-oriented implementation.
7. Unit and integration tests.
8. Design tradeoff document.
9. README and execution instructions.

Don't just submit code. Submit your design reasoning.

## 12. LLD Interview Preparation – Think Like an Engineer

Here are some frequently used LLD practice problems.

| Problem             | Important concepts                 |
| ------------------- | ---------------------------------- |
| LRU Cache           | HashMap, Doubly Linked List        |
| Notification System | Strategy, Factory, Observer        |
| Rate Limiter        | Token Bucket, Sliding Window       |
| Tic-Tac-Toe         | Object modeling, State             |
| Snake and Ladder    | Composition, Game State            |
| Logger System       | Chain of Responsibility            |
| Parking Lot         | Allocation, Strategy               |
| Elevator System     | Scheduling, State                  |
| ATM                 | State, Transaction handling        |
| Food Ordering       | Order lifecycle, Payment, Strategy |

### How should you approach an LLD interview?

Do not immediately start coding.

Follow this sequence:

```
        PROBLEM STATEMENT
                |
                v
       REQUIREMENT ANALYSIS
                |
                v
        IDENTIFY ENTITIES
                |
                v
       DEFINE RESPONSIBILITIES
                |
                v
        DEFINE INTERFACES
                |
                v
       DESIGN CLASS DIAGRAM
                |
                v
      DESIGN INTERACTION FLOW
                |
                v
        APPLY DESIGN PATTERNS
          WHERE NEEDED
                |
                v
       IMPLEMENT AND TEST
                |
                v
       DISCUSS TRADEOFFS
```

During an interview, explain:

- Why did you create this class?
- Why did you introduce this interface?
- Why did you choose composition instead of inheritance?
- How will your design support a new feature?
- What happens when an operation fails?
- How will you test this class?
- What happens if two users perform the same operation simultaneously?

These questions reveal engineering maturity.

## 13. Transflower LLD Learning Roadmap

Let's organize everything into a practical learning journey.

## LLD Mastery – 8 Learning Stages

1. Stage 1 – OOP Fundamentals

   Classes, objects, abstraction, encapsulation, inheritance and polymorphism.

   Foundation
2. Stage 2 – SOLID Principles

   SRP, OCP, LSP, ISP and DIP.

   Design thinking
3. Stage 3 – Design Patterns

   Creational, structural and behavioral patterns.

   Reusable solutions
4. Stage 4 – UML and Modeling

   Class, sequence, activity and object diagrams.

   Visual design
5. Stage 5 – Interfaces and Dependency Injection

   Abstraction, loose coupling, IoC and composition.

   Extensibility
6. Stage 6 – Clean Code and TDD

   Refactoring, unit testing, mocking and code smells.

   Quality engineering
7. Stage 7 – Real-World LLD Projects

   Parking lot, ATM, BookMyShow, Elevator and Notification System.

   Application
8. Stage 8 – Interview Simulation

   Timed design exercises, coding, reviews and tradeoff discussions.

   Employability

## 14. Transflower Classroom Assignment

Mentor Assessment

Problem Statement: Design a Student Assessment System using LLD.

Your application should support:

- Student registration.
- Assessment creation.
- Question management.
- MCQ evaluation.
- Coding evaluation.
- Result generation.
- Notification.

### Student deliverables

| Deliverable                | Marks |
| -------------------------- | ----- |
| Requirement analysis       | 10    |
| Object identification      | 10    |
| OOP implementation         | 15    |
| SOLID principles           | 15    |
| Design pattern application | 10    |
| UML diagrams               | 15    |
| Dependency injection       | 10    |
| Unit testing               | 10    |
| Documentation              | 5     |
| Total                      | 100   |

### Mentor evaluation criteria

Don't award marks simply because students have created many classes.

Evaluate whether:

- Responsibilities are clear.
- Classes are cohesive.
- Dependencies are manageable.
- New assessment types can be added easily.
- Business logic is testable.
- Error conditions are handled.
- Design decisions can be explained.

 

## 15. Mentor's Closing Message

Dear students, There is a difference between knowing a programming language and understanding software design. You may know:

- Java syntax.
- C# syntax.
- Python syntax.
- JavaScript syntax.

But that alone does not make you a software engineer. A software engineer understands:

- How to model a problem.
- How to identify objects.
- How to assign responsibilities.
- How to manage dependencies.
- How to design for change.
- How to write testable code.

LLD is not about writing 500 lines of code. It is about making the right design decisions before writing those 500 lines.

Remember this engineering journey:

```
Programming Fundamentals
          |
          v
       OOP
          |
          v
        SOLID
          |
          v
  Design Patterns
          |
          v
         UML
          |
          v
   Low-Level Design
          |
          v
   Clean Architecture
          |
          v
  Production Software
```

HLD helps you see the whole system. LLD helps you understand the individual building blocks. Coding brings those building blocks to life. Testing gives us confidence. And engineering discipline makes the system maintainable. Don't just become a framework developer. Become a problem-solving software engineer.

At Transflower, we don't teach students merely to write code. We mentor them to think, design, build, test and evolve software systems. 