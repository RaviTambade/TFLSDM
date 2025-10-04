# Automation in Tseting along with Cucumber

Last time, we sat together and said:
“Let’s not just jump into writing tests randomly. Let’s build the *foundation* first.” That’s why we focused on **structure** – setting up folders, arranging packages, making sure that when we *do* start testing, we don’t end up with a messy kitchen where the utensils are everywhere and no one knows where the spoon is!

### 🌱 Today’s Next Step: Bringing in BDD

Now, imagine you and I are chefs again, but this time we’re not cooking just for ourselves. We’re cooking for customers who don’t know the recipe but definitely know what the food should taste like.

That’s exactly what **Behavior Driven Development (BDD)** does in software.

* The **customer (or business analyst)** says: *“When I click on checkout, I should see a confirmation screen.”*
* Instead of writing complex Java test cases upfront, we capture this in **simple English**:

```gherkin
Scenario: Successful checkout
  Given I have items in my cart
  When I click on checkout
  Then I should see a confirmation message
```

See how natural that sounds? That’s the beauty of BDD.

### ⚡ Why this matters for you as a developer

You don’t just write tests. You’re now **connecting the business requirement → to the test → to the code.**
And the best part? Once you write this `.feature` file, tools like **Cucumber + TestNG + Maven** will automatically match it with the underlying code and run it for you when you execute:

```bash
mvn test
```

Suddenly, your plain English test becomes **automated verification**.


### 🔄 Automation Insight

Now let me connect it with your thought on automation.
In software today, we’re in a generation where repetitive things should never be typed again and again. Tools like **GitHub Copilot** or IntelliJ AI are like your kitchen helpers. If you want to cook dal, they already know how to chop onions, add turmeric, and boil lentils.

* Similarly, if you want to write CRUD operations (Insert, Update, Delete, GetById), these tools will scaffold the code for you.
* Your job is not just to accept it blindly, but to **understand the flow**:

  * How the `insert` talks to your database
  * How the `get` returns data in JSON
  * How tests verify that everything is consistent

This means your focus shifts from just typing → to **thinking about operations**.

### 🧭 Where we are headed

So in today’s journey:

1. We’ll **write a feature file** in English describing a simple CRUD operation for, say, a *Product table*.
2. We’ll see how Cucumber converts it into runnable test steps.
3. We’ll tie it into our codebase so that every time we run `mvn test`, these scenarios are executed automatically.

That’s not just testing. That’s **bridging requirements and code**.
And honestly, this is where you start becoming not just a *coder*, but a **professional engineer** who can speak both business and technical language.


## 🌱 Storytelling Mentor’s Way

Imagine we are standing in front of a giant factory.

* At the **ground floor**, we have machines, electricity, and wiring → this is your **infrastructure level**.
* Then comes the **assembly line floor**, where machines are configured and operated in an organized manner → that’s your **platform level**.
* Finally, at the **top floor**, we have finished products (like Word, Teams, Notepad) → these are your **applications (SaaS)**.

Now let’s walk through the story step by step.


### 1️⃣ SDLC – The Factory Blueprint

The **Software Development Life Cycle (SDLC)** is like planning how the factory will produce a new car.

* You need **requirements** (design sketch),
* **design** (blueprints),
* **development** (assembling),
* **testing** (quality check),
* **deployment** (delivering cars to showrooms),
* and finally **maintenance** (servicing cars).

This cycle keeps repeating with every new release.


### 2️⃣ Infrastructure Level – Virtualization & Cloud

Earlier, to run software, we only had **physical servers**. That’s like having one giant machine in the factory for one purpose only. But what if you want to run many different processes on the same machine?

👉 Enter **virtualization**.

* Hyper-V, VMware, or KVM act as **hypervisors**.
* They let you cut one physical server into many **virtual machines**, each behaving like an independent machine.

Later, this extended into **cloud** (AWS, Azure, GCP). Now you don’t even need to buy machines. You just “rent” them as **Infrastructure as a Service (IaaS)**.

Automation here = spinning up servers, networks, storage without touching hardware.



### 3️⃣ Platform Level – Environment for Developers

On top of infrastructure, you need a **platform**:

* OS, runtime (JVM, .NET CLR, Node.js),
* tools like Kubernetes, Docker, Red Hat OpenShift.

This **Platform as a Service (PaaS)** makes sure you don’t waste time managing hardware. You directly deploy your applications.
Automation here = scaling up pods, managing load balancers, and ensuring high availability.



### 4️⃣ Application Level – Software for End Users

Here comes the final product → **Applications (SaaS)**.

* Word, PowerPoint, Teams, Gmail, Online Shopping apps.
* Built using architectures: Monoliths → Distributed Apps → Microservices.
* Deployed on Kubernetes clusters, scaled across Mumbai, Bangalore, US data centers.

Automation here = CI/CD pipelines (build, test, deploy automatically).


### 5️⃣ Where Does **BDD** Fit?

Now, think about the **Testing phase** inside SDLC. Traditionally, testers wrote long, complex scripts. But today’s world moves fast → Agile, DevOps, sprints. You don’t have time for manual translations between *business requirements* and *test scripts*.

👉 That’s where **BDD (Behavior Driven Development)** shines:

* Business Analyst says: *“Order checkout should show confirmation.”*
* We capture it in **plain English Gherkin syntax**.
* Automation tools (Cucumber, SpecFlow) link it to test code.
* Every time CI/CD pipeline runs, these tests execute automatically.

Automation here = bridging **business requirements → automated tests → production-ready applications**.



### 6️⃣ The Grand Picture 🌍

* **Infrastructure automation** = Virtualization + Cloud (IaaS)
* **Platform automation** = Kubernetes, Docker, CI/CD (PaaS)
* **Application automation** = SaaS deployment + BDD-driven testing

So when you talk about **automation**, don’t just think of one layer. Automation is like electricity running through all floors of the factory.

And when you talk about **BDD**, it’s like the quality inspector in the factory who understands customer language and makes sure the cars being produced actually match the customer’s expectations — without waiting till the end.

 