🌼 ** Welcome to Transflower Docker Desktop**


Good morning everyone ☀️

Yesterday we understood:

* What is **Cloud Computing**
* What is **Containerization**
* Why containers are important in modern application deployment

Today is not theory.
Today is **execution day**.


# 🎯 Today’s Mission

Different technology students → Same DevOps mindset.

* Java developers → Build and containerize a **Spring Boot API**
* Node.js developers → Containerize a simple **Express application**
* .NET developers → Deploy existing **ASP.NET Core Web API** as a containerized microservice

Same pattern.
Different ecosystems.
One DevOps discipline.



# 🧠 First Understanding – Where Will You Run?

Most of you may not have AWS account.
Most of you may not have Azure subscription.

So we have **two choices**:


## 🔹 Choice 1 – Public Cloud (Real World Exposure)

You can create free-tier accounts on:

* Amazon Web Services
* Microsoft Azure
* Google Cloud

You will get:

* Free tier (approx 750–760 hours per month for VM)
* Small credit card/UPI verification (₹2 temporary authorization)
* Access to:

  * Infrastructure as a Service (VM)
  * Platform as a Service
  * Container services

⚠️ That ₹2 deduction is not a charge.
It is identity verification. It gets reversed.

Once registered, you can:

* Create virtual machine
* Install Docker
* Deploy your container
* Access from remote browser

That is real cloud experience.


## 🔹 Choice 2 – Local Machine (Learning Mode)

If your machine has:

* Windows 11
* Minimum 8 GB RAM
* Enough disk space

Then you can install:

## 👉 Docker Desktop


# 💻 Installing Docker Desktop (Windows)

Step 1:
Go to Google → Search:

```
Docker Desktop for Windows
```

Step 2:
Download installer (approx 600–630 MB)

Step 3:
Install normally (Next → Next → Finish)

After installation:

* Go to Start Menu
* Type: `Docker Desktop`
* Open the application

You will see:

* Containers running
* Images available
* Volumes
* Dashboard UI

This is your **local container lab environment**.


# 🐧 If You Are Using Linux

If you are using Ubuntu:

```bash
sudo apt update
sudo apt install docker.io
```

Docker Engine will be installed.

Then verify:

```bash
docker --version
docker ps
```


# 🏗 Today’s Hands-on Plan



## 🟢 Java Students

1. Create Spring Boot API
2. Create Dockerfile
3. Build Image
4. Run container
5. Access via browser

Outcome:
You understand JVM inside container.



## 🟢 Node.js Students

1. Create simple Express app
2. Add Dockerfile
3. Build image
4. Run container
5. Test endpoint

Outcome:
You understand Node runtime inside container.



## 🟢 .NET Students

1. Take existing ASP.NET Web API
2. Add multi-stage Dockerfile
3. Build image
4. Run container
5. Deploy as microservice

Outcome:
You understand CLR inside container.


# 🧠 Important Mindset

Today is not about:

* Java vs .NET vs Node

Today is about:

> Runtime independence + Container standardization

Because Docker does not care:

* Whether it is JVM
* Whether it is CLR
* Whether it is V8 engine

Container is neutral.


# 🏗 What You Will Realize Today

You will see:

```text
Source Code
    ↓
Dockerfile
    ↓
docker build
    ↓
Image
    ↓
docker run
    ↓
Container (Running Service)
```

This flow is same for all technologies.



# 🔥 Bigger Vision

Once you can:

* Build image
* Run container
* Access via browser

Then next step is:

* Deploy to Cloud VM
* Push image to Docker Hub
* Deploy to Kubernetes

That is how modern companies work.


# 🧭 Mentor Reflection

Yesterday you understood:

> Cloud is someone else's computer.

Today you will understand:

> Container is portable execution unit.

And tomorrow you will understand:

> Microservices are independently deployable business capabilities.

 


# 🔹 What is a Dockerfile?

A **Dockerfile** is a text file that contains instructions to build a Docker image.

When you run:

```bash
docker build -t myapp .
```

Docker reads the Dockerfile step-by-step and creates image layers.


# 🔥 Most Important Dockerfile Commands (With Meaning)



## 1️⃣ `FROM`

### 👉 Meaning:

Defines the **base image** (foundation OS + runtime).

Without `FROM`, Dockerfile cannot start.

### Example:

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
```

💡 Meaning:

> Start building my image using .NET 8 ASP.NET runtime image.

Think of it as:

> "Start my house on this land."


## 2️⃣ `WORKDIR`

### 👉 Meaning:

Sets the working directory inside the container.

If folder doesn’t exist, Docker creates it.

### Example:

```dockerfile
WORKDIR /app
```

💡 Meaning:

> From now on, all commands run inside `/app`.



## 3️⃣ `COPY`

### 👉 Meaning:

Copies files from your local machine → into container.

### Example:

```dockerfile
COPY . .
```

💡 Meaning:

> Copy everything from current folder into container working directory.

More specific:

```dockerfile
COPY publish/ .
```


## 4️⃣ `ADD` (Similar to COPY but Advanced)

### 👉 Meaning:

Copies files and can:

* Extract .tar files automatically
* Download from URL

⚠️ Best practice:
Use `COPY` unless you need extra features.


## 5️⃣ `RUN`

### 👉 Meaning:

Executes command **while building image**.

Used for:

* Installing packages
* Restoring dependencies
* Building application

### Example:

```dockerfile
RUN dotnet restore
RUN dotnet build
```

💡 Meaning:

> Execute this command and save result as a new image layer.

Important:
`RUN` executes at **build time**, not at container runtime.


## 6️⃣ `CMD`

### 👉 Meaning:

Defines default command when container starts.

### Example:

```dockerfile
CMD ["dotnet", "MyApp.dll"]
```

💡 Meaning:

> When container runs, execute this.

Important:
Only **one CMD** is allowed (last one wins).


## 7️⃣ `ENTRYPOINT`

### 👉 Meaning:

Defines main executable for container.

Difference from CMD:

* ENTRYPOINT = fixed
* CMD = default argument

### Example:

```dockerfile
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

Think:

> ENTRYPOINT = Main function
> CMD = Default parameters



## 8️⃣ `EXPOSE`

### 👉 Meaning:

Documents which port container listens on.

### Example:

```dockerfile
EXPOSE 8080
```

💡 Meaning:

> This container will use port 8080 internally.

⚠️ It does NOT publish port.
Publishing happens using:

```bash
docker run -p 5000:8080 myimage
```


## 9️⃣ `ENV`

### 👉 Meaning:

Sets environment variables.

### Example:

```dockerfile
ENV ASPNETCORE_ENVIRONMENT=Production
```

Used for:

* Configuration
* Connection strings
* Secrets (not recommended for secrets in production)



## 🔟 `ARG`

### 👉 Meaning:

Defines build-time variable.

### Example:

```dockerfile
ARG VERSION=1.0
```

Used during build:

```bash
docker build --build-arg VERSION=2.0 .
```

Difference:

| ARG                       | ENV                        |
| ------------------------- | -------------------------- |
| Build time                | Runtime                    |
| Not available after build | Available inside container |



## 1️⃣1️⃣ `USER`

### 👉 Meaning:

Defines which user runs inside container.

Example:

```dockerfile
USER appuser
```

Security best practice:
Do NOT run as root.



## 1️⃣2️⃣ `VOLUME`

### 👉 Meaning:

Creates mount point for persistent storage.

```dockerfile
VOLUME /data
```

Used for:

* Database storage
* Logs
* Persistent files



# 🔥 Complete Flow Example (.NET Web API)

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY publish/ .
EXPOSE 8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```



# 🧠 Build-Time vs Run-Time Understanding

| Build Time | Run Time   |
| ---------- | ---------- |
| FROM       | CMD        |
| RUN        | ENTRYPOINT |
| COPY       | ENV        |
| ARG        | USER       |

This clarity is very important when teaching students.



# 🏗 Mentor Analogy (Your Style)

Think Dockerfile like:

| Real World                | Docker  |
| ------------------------- | ------- |
| Land                      | FROM    |
| House Room                | WORKDIR |
| Bring furniture           | COPY    |
| Install AC                | RUN     |
| Main Door                 | EXPOSE  |
| Start TV when house opens | CMD     |
| House Owner               | USER    |


# 🐳 Where Is Docker Image Stored on Windows?

It depends on **which Docker engine you are using**.

Most Windows developers today use:

## 👉 Docker Desktop

And Docker Desktop uses:

* **WSL2 backend (default)**
* Or Hyper-V (older setup)


# ✅ If Using WSL2 (Default in Modern Windows)

Your Docker images are **NOT stored directly in C:\ drive in normal folders**.

They are stored inside a **virtual Linux filesystem** managed by WSL.

### Actual Physical Location:

```text
C:\Users\<your-username>\AppData\Local\Docker\wsl\data\ext4.vhdx
```

### What is this?

`ext4.vhdx` = A virtual hard disk file
It contains:

* Docker images
* Containers
* Volumes
* Networks

⚠️ You cannot browse image files normally like `.jpg` or `.exe`.

Docker stores images in **layered format** inside this virtual disk.



# 🔍 If You Want to See Docker Image Data Internally

Open PowerShell:

```bash
docker info
```

Look for:

```text
Docker Root Dir
```

Inside WSL, it usually shows:

```text
/var/lib/docker
```

That is the **Linux internal path** where images are stored.



# 🐳 If Using Windows Containers (Not Linux Containers)

Then location may be:

```text
C:\ProgramData\Docker
```

But again, not readable as simple files.



# 🧠 Important Understanding

Docker Image is NOT a single file.

It is:

* Multiple layers
* Stored in content-addressable storage
* Managed by Docker Engine

You cannot open it like:

```text
myimage.docker
```

Because it doesn't exist like that.


# 📦 If You Want Image As a File

You must export it manually:

```bash
docker save -o myimage.tar myimage:latest
```

Now you will get:

```text
myimage.tar
```

This file you can move or share.

To load again:

```bash
docker load -i myimage.tar
```


# 🏗 Architecture Understanding (Mentor Mode)

On Windows:

```
Windows OS
   ↓
Docker Desktop
   ↓
WSL2 Linux VM
   ↓
/var/lib/docker
   ↓
Image Layers
```

So image is inside Linux virtual machine — not directly inside Windows file explorer.



# 🔥 Best Practice

- 👉 Do NOT manually modify files inside Docker storage.
- 👉 Always use Docker CLI commands.


# 🧠 Big Idea First

All 3 follow the same container pattern:

```text
Base Image (Runtime)
        ↓
Working Directory
        ↓
Copy Source
        ↓
Install Dependencies
        ↓
Expose Port
        ↓
Start Application
```

But the **runtime ecosystem** differs.


# 1️⃣ .NET Web API Dockerfile

Framework: ASP.NET Core

## ✅ Multi-Stage (Production Ready)

```dockerfile
# Stage 1: Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

# Stage 2: Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

### 🔎 Understanding

* SDK image → used for building
* ASP.NET runtime → used for running
* Final image is smaller (no SDK inside)


# 2️⃣ Spring Boot Dockerfile

Framework: Spring Boot

## ✅ Multi-Stage (Maven Based)

```dockerfile
# Stage 1: Build
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Runtime
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=build /app/target/myapp.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

### 🔎 Understanding

* Maven builds `.jar`
* JVM runs `.jar`
* Java runtime required



# 3️⃣ Python Web API Dockerfile

Framework Example: FastAPI
(or Django)

## ✅ Simple Version

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

### 🔎 Understanding

* Python interpreter included in base image
* pip installs dependencies
* Uvicorn runs server



# 🔥 Side-by-Side Comparison

| Feature             | .NET           | Spring Boot  | Python             |
| ------------------- | -------------- | ------------ | ------------------ |
| Build Tool          | dotnet CLI     | Maven/Gradle | pip                |
| Output              | DLL            | JAR          | Python scripts     |
| Runtime             | dotnet runtime | JVM          | Python interpreter |
| Default Port        | 8080           | 8080         | 8000               |
| Multi-stage Needed? | Yes            | Yes          | Optional           |
| Image Size          | Medium         | Larger (JVM) | Smaller (slim)     |

---

# 🏗 Architecture Difference (Mentor View)

## .NET

```text
dotnet build → DLL → dotnet runtime executes DLL
```

## Java

```text
maven build → JAR → JVM executes JAR
```

## Python

```text
pip install → Python interpreter runs script directly
```


# 🧠 Runtime Philosophy Difference

| Ecosystem | Execution Model           |
| --------- | ------------------------- |
| .NET      | Compiled → IL → Runtime   |
| Java      | Compiled → Bytecode → JVM |
| Python    | Interpreted at runtime    |



# 🔥 Image Size Comparison (Typical)

| Framework   | Approx Image Size |
| ----------- | ----------------- |
| .NET        | 200–300 MB        |
| Spring Boot | 300–500 MB        |
| Python      | 100–200 MB        |

(Java is heavier because of JVM.)



# 🎯 Enterprise Best Practices (All 3)

Always:

```dockerfile
USER appuser
ENV ASPNETCORE_ENVIRONMENT=Production
```

Use:

* Multi-stage builds
* Non-root user
* Minimal base image (alpine/slim)

# 🏫 Classroom Whiteboard View

Draw this:

```text
               Dockerfile
                   ↓
-------------------------------------
| .NET | Java | Python |
-------------------------------------
| SDK  | Maven | pip   |
| DLL  | JAR   | .py   |
| CLR  | JVM   | Python|
-------------------------------------
```

Students then understand:

> Docker pattern is SAME.
> Runtime ecosystem is DIFFERENT.



This lab will help them understand:

* Dockerfile
* Multi-stage builds
* Image layers
* Container runtime
* Port mapping
* Logs
* Image export/import
* Comparison: .NET vs Spring Boot vs Python



# 🧪 LAB TITLE

# 🐳 Containerizing Web APIs – .NET vs Spring Boot vs Python



# 🎯 LAB OBJECTIVES

By the end of this lab, students will:

1. Build Docker image
2. Run container
3. Understand build-time vs run-time
4. Compare runtime ecosystems
5. Export and import images
6. Debug container issues



# 🛠 PREREQUISITES

* Install Docker Desktop
* Enable WSL2
* Verify:

```bash
docker --version
docker info
```



# 🏗 LAB STRUCTURE

We will create **three simple APIs**:

1. .NET Web API – `/hello`
2. Spring Boot API – `/hello`
3. Python FastAPI – `/hello`

All return:

```json
{ "message": "Hello from Container" }
```



# 🔹 PART 1 – .NET API LAB



Framework: ASP.NET Core



## Step 1 – Create Project

```bash
dotnet new webapi -n DotnetApi
cd DotnetApi
```

Modify `Program.cs`:

```csharp
app.MapGet("/hello", () => new { message = "Hello from Container" });
```

Test locally:

```bash
dotnet run
```


## Step 2 – Create Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "DotnetApi.dll"]
```



## Step 3 – Build Image

```bash
docker build -t dotnetapi .
```


## Step 4 – Run Container

```bash
docker run -p 5001:8080 dotnetapi
```

Test:

```
http://localhost:5001/hello
```



# 🔹 PART 2 – Spring Boot LAB


Framework: Spring Boot


## Step 1 – Create Project

Using Spring Initializr (Maven + Java 17)

Add controller:

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public Map<String,String> hello() {
        return Map.of("message","Hello from Container");
    }
}
```



## Step 2 – Dockerfile

```dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```



## Step 3 – Build Image

```bash
docker build -t springapi .
```



## Step 4 – Run

```bash
docker run -p 5002:8080 springapi
```

Test:

```
http://localhost:5002/hello
```


# 🔹 PART 3 – Python FastAPI LAB

---

Framework: FastAPI

---

## Step 1 – Create `main.py`

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/hello")
def hello():
    return {"message": "Hello from Container"}
```

Create `requirements.txt`

```
fastapi
uvicorn
```

---

## Step 2 – Dockerfile

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```


## Step 3 – Build Image

```bash
docker build -t pythonapi .
```

---

## Step 4 – Run

```bash
docker run -p 5003:8000 pythonapi
```

Test:

```
http://localhost:5003/hello
```

---

# 🔥 PART 4 – Comparison Exercise

Ask students:

1. Which image is biggest?

   ```bash
   docker images
   ```

2. Inspect image layers:

   ```bash
   docker history dotnetapi
   ```

3. See running containers:

   ```bash
   docker ps
   ```

4. View logs:

   ```bash
   docker logs <container_id>
   ```

---

# 🔁 PART 5 – Export & Import

Export:

```bash
docker save -o dotnetapi.tar dotnetapi
```

Delete image:

```bash
docker rmi dotnetapi
```

Import:

```bash
docker load -i dotnetapi.tar
```


# 🧠 Reflection Questions (Important)

1. Why do we use multi-stage builds?
2. Why Java image is heavier?
3. What is difference between CMD and ENTRYPOINT?
4. What happens if EXPOSE is removed?
5. Where are images stored internally?


# 🏗 Mentor Whiteboard Explanation

```text
Developer Code
      ↓
Dockerfile
      ↓
docker build
      ↓
Docker Image (Layered)
      ↓
docker run
      ↓
Container (Running Process)
```

---

# 🧪 BONUS CHALLENGE (Advanced Students)

1. Add Environment Variable
2. Run container as non-root user
3. Reduce image size
4. Push image to Docker Hub
5. Run multiple containers with different ports

---

# 🎓 Learning Outcome

Students will understand:

* Container ≠ VM
* Image = Blueprint
* Dockerfile = Automation Script
* Runtime ecosystems differ
* DevOps bridge between Developer and Ops

 

# 🧪 LAB ANSWER KEY

**Topic:** Dockerizing Web APIs


# ✅ PART 1 – .NET API (ASP.NET Core)

Framework: ASP.NET Core

---

## ✔ Expected Output

### Build

```bash
docker build -t dotnetapi .
```

✔ Should complete without errors
✔ Image appears in:

```bash
docker images
```

Expected entry:

```
REPOSITORY   TAG       SIZE
dotnetapi    latest    ~200-300MB
```

---

### Run

```bash
docker run -p 5001:8080 dotnetapi
```

✔ Container starts
✔ Accessible at:

```
http://localhost:5001/hello
```

Expected response:

```json
{ "message": "Hello from Container" }
```

---

## ✔ Concept Check Answers

**Q: Why multi-stage build?**
👉 To reduce final image size by removing SDK layer.

**Q: What runs inside container?**
👉 dotnet runtime executing DLL.

**Q: If EXPOSE removed?**
👉 Container still works. EXPOSE is documentation only.

---

# ✅ PART 2 – Spring Boot

Framework: Spring Boot

---

## ✔ Expected Output

### Build

```bash
docker build -t springapi .
```

Image size:

```
~300-500MB
```

(Heavier due to JVM)

---

### Run

```bash
docker run -p 5002:8080 springapi
```

Test:

```
http://localhost:5002/hello
```

Expected:

```json
{ "message": "Hello from Container" }
```

---

## ✔ Concept Check Answers

**Q: Why larger image?**
👉 JVM runtime included.

**Q: What runs inside container?**
👉 JVM running app.jar

**Q: What is jar?**
👉 Compiled Java bytecode package.

---

# ✅ PART 3 – Python FastAPI

Framework: FastAPI

---

## ✔ Expected Output

### Build

```bash
docker build -t pythonapi .
```

Image size:

```
~100-200MB
```

---

### Run

```bash
docker run -p 5003:8000 pythonapi
```

Test:

```
http://localhost:5003/hello
```

Expected:

```json
{ "message": "Hello from Container" }
```

---

# 🔥 PART 4 – Comparison Answers

| Feature      | .NET   | Spring Boot | Python      |
| ------------ | ------ | ----------- | ----------- |
| Runtime      | CLR    | JVM         | Interpreter |
| Output       | DLL    | JAR         | .py         |
| Build Tool   | dotnet | Maven       | pip         |
| Image Size   | Medium | Large       | Small       |
| Startup Time | Fast   | Slower      | Fast        |

---

# 🔍 Docker Command Answers

---

## ✔ `docker ps`

Shows running containers.

Expected:

```
CONTAINER ID   IMAGE        PORTS
xxxx           dotnetapi    0.0.0.0:5001->8080
```

---

## ✔ `docker logs <container_id>`

Shows application logs.

---

## ✔ `docker history dotnetapi`

Shows image layers.

Expected understanding:

* Each Dockerfile instruction = Layer
* Layers are cached

---

# 🔁 Export & Import Answers

---

## ✔ Save Image

```bash
docker save -o dotnetapi.tar dotnetapi
```

File created:

```
dotnetapi.tar
```

---

## ✔ Remove Image

```bash
docker rmi dotnetapi
```

---

## ✔ Load Image

```bash
docker load -i dotnetapi.tar
```

Image restored successfully.

---

# 🧠 Deep Concept Answers (Important for Viva)

---

### Q1: Difference Between Image and Container?

Image = Blueprint
Container = Running instance of image

---

### Q2: Build-time vs Run-time?

| Build-time | Run-time   |
| ---------- | ---------- |
| FROM       | CMD        |
| RUN        | ENTRYPOINT |
| COPY       | ENV        |

---

### Q3: What happens internally when docker run executes?

1. Creates writable layer
2. Allocates network
3. Maps port
4. Starts process
5. Attaches logs

---

### Q4: Where images stored on Windows?

Inside WSL2 virtual disk:

```
ext4.vhdx
```

Managed by Docker Desktop

---

### Q5: Why container ≠ VM?

| VM           | Container          |
| ------------ | ------------------ |
| Full OS      | Shares host kernel |
| Heavy        | Lightweight        |
| Slow startup | Fast startup       |

---

# 🏆 Grading Rubric

| Criteria               | Marks |
| ---------------------- | ----- |
| Dockerfile correctness | 20    |
| Build success          | 20    |
| Run success            | 20    |
| Concept explanation    | 20    |
| Debugging ability      | 20    |

Total: 100

 

#### If a student understands this lab deeply,
they understand:

* Runtime architecture
* DevOps pipeline bridge
* Container lifecycle
* Build vs Run concept
* Image layering

This is industry-ready thinking.


 
# 🧩 LAB TITLE

# Multi-Container Microservices using Docker Compose

   

# 🎯 LAB OBJECTIVE

Students will learn:

* Microservices architecture
* Service-to-service communication
* Docker networks
* Docker Compose
* Environment variables
* Container dependency management

---

# 🏗 Architecture Overview

We will build:

1. Product Service (.NET)
2. Order Service (Spring Boot)
3. API Gateway (Python)
4. Redis (Cache)

Frameworks used:

* ASP.NET Core
* Spring Boot
* FastAPI
* Redis
* Docker Compose

---

# 🧠 System Flow

```text
Client
   ↓
API Gateway (Python - Port 8000)
   ↓
-------------------------------------
|           Docker Network          |
-------------------------------------
   ↓                     ↓
Product Service      Order Service
(.NET - 5001)        (Java - 5002)
          ↓
        Redis
```

---

# 📁 Project Structure

```text
microservices-lab/
│
├── product-service/
├── order-service/
├── api-gateway/
└── docker-compose.yml
```

---

# 🔹 PART 1 – Product Service (.NET)

### Endpoint

```text
GET /products
```

Returns:

```json
[
  { "id": 1, "name": "Laptop" }
]
```

### Dockerfile (Multi-stage)

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app/publish

FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "ProductService.dll"]
```

---

# 🔹 PART 2 – Order Service (Spring Boot)

### Endpoint

```text
GET /orders
```

Returns:

```json
[
  { "id": 101, "productId": 1 }
]
```

### Dockerfile

```dockerfile
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

---

# 🔹 PART 3 – API Gateway (Python FastAPI)

Acts as aggregator.

### Endpoints:

```text
GET /products
GET /orders
GET /dashboard   ← calls both services
```

Inside Python:

```python
import requests
from fastapi import FastAPI

app = FastAPI()

PRODUCT_URL = "http://product-service:8080/products"
ORDER_URL = "http://order-service:8080/orders"

@app.get("/dashboard")
def dashboard():
    products = requests.get(PRODUCT_URL).json()
    orders = requests.get(ORDER_URL).json()
    return {"products": products, "orders": orders}
```

Notice:

👉 `product-service` and `order-service`
These are container names (DNS inside Docker network).

---

# 🔹 Dockerfile for API Gateway

```dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# 🔥 PART 4 – Docker Compose File

`docker-compose.yml`

```yaml
version: '3.9'

services:

  product-service:
    build: ./product-service
    ports:
      - "5001:8080"

  order-service:
    build: ./order-service
    ports:
      - "5002:8080"

  api-gateway:
    build: ./api-gateway
    ports:
      - "8000:8000"
    depends_on:
      - product-service
      - order-service

  redis:
    image: redis:7
    ports:
      - "6379:6379"
```

---

# 🚀 Running Entire System

```bash
docker compose up --build
```

Test:

```text
http://localhost:8000/dashboard
```

Expected:

```json
{
  "products": [...],
  "orders": [...]
}
```

---

# 🧠 Key Learning Concepts

---

## 1️⃣ Internal Docker Networking

Containers talk using:

```text
http://service-name:port
```

NOT localhost.

---

## 2️⃣ Service Discovery

Docker Compose provides:

* Automatic DNS
* Shared network

---

## 3️⃣ Dependency Management

```yaml
depends_on:
```

Controls startup order (not health check).

---

## 4️⃣ Microservice Isolation

Each service:

* Own Dockerfile
* Own runtime
* Independent scaling

---

# 🧪 Lab Exercises for Students

1. Break product service → see gateway failure.
2. Add Redis caching to product service.
3. Scale product service:

   ```bash
   docker compose up --scale product-service=3
   ```
4. Add environment variables.
5. Implement health check endpoint.

---

# 🎯 Reflection Questions

1. Why use API Gateway?
2. Why not call database directly from gateway?
3. What happens if one service crashes?
4. Difference between container network and localhost?
5. How would this scale in Kubernetes?

---

# 🏆 Advanced Extension

Next evolution:

* Add PostgreSQL container
* Add RabbitMQ for async communication
* Convert to Kubernetes
* Implement distributed tracing

---

# 🧠 Mentor Insight

This lab transforms students from:

> "Docker command learners"

to

> "Distributed system thinkers"

They now understand:

* Service boundaries
* Runtime diversity
* Inter-container networking
* Infrastructure automation
 
 

# 🧩 25 Microservices Viva Questions with Answers

---

## 🔹 1. What is Microservices Architecture?

Microservices is an architectural style where an application is built as a collection of small, independent services that communicate over a network.

Each service:

* Has its own business responsibility
* Can be deployed independently
* Owns its own data

---

## 🔹 2. How is Microservices different from Monolith?

| Monolith                | Microservices                |
| ----------------------- | ---------------------------- |
| Single deployable unit  | Multiple deployable units    |
| Shared database         | Independent databases        |
| Hard to scale partially | Scale services independently |
| Tight coupling          | Loose coupling               |

---

## 🔹 3. What are key characteristics of Microservices?

* Independent deployment
* Decentralized data management
* Lightweight communication (REST, gRPC)
* Fault isolation
* DevOps-friendly

---

## 🔹 4. What is API Gateway?

An API Gateway is a single entry point for clients.

Responsibilities:

* Routing
* Authentication
* Rate limiting
* Aggregation

Example tools:

* NGINX
* Kong

---

## 🔹 5. Why do we need Service Discovery?

Because service IP addresses change dynamically.

Service discovery helps services locate each other.

Example:

* Eureka
* Consul

---

## 🔹 6. What is Database per Service?

Each microservice has its own database.

Why?

* Loose coupling
* Independent schema evolution
* Independent scaling

---

## 🔹 7. What is Synchronous Communication?

Direct request-response communication.

Example:

* REST
* gRPC

Used when immediate response is required.

---

## 🔹 8. What is Asynchronous Communication?

Message-based communication.

Example:

* RabbitMQ
* Apache Kafka

Used for:

* Event-driven systems
* Loose coupling
* Better scalability

---

## 🔹 9. What is Event-Driven Architecture?

Services communicate via events.

Example:

* OrderPlaced event
* PaymentCompleted event

Producers emit events → consumers react.

---

## 🔹 10. What is Circuit Breaker Pattern?

Prevents cascading failures.

If service is down:

* Stop sending requests
* Return fallback response

Example:

* Resilience4j

---

## 🔹 11. What is Saga Pattern?

Manages distributed transactions across services.

Two types:

* Choreography (event-based)
* Orchestration (central coordinator)

Used when:
Multiple services must maintain consistency.

---

## 🔹 12. What is Idempotency?

Making the same request multiple times produces the same result.

Example:
Payment API with transaction ID.

---

## 🔹 13. What is Containerization in Microservices?

Each service runs inside a container.

Example:

* Docker

Benefits:

* Environment consistency
* Lightweight deployment
* Fast scaling

---

## 🔹 14. What is Orchestration?

Managing multiple containers.

Example:

* Kubernetes

Handles:

* Auto scaling
* Self-healing
* Rolling updates

---

## 🔹 15. What is Load Balancing?

Distributing traffic across multiple service instances.

Example:

* HAProxy

---

## 🔹 16. What is Horizontal Scaling?

Increasing number of service instances.

Example:
3 replicas of product service.

---

## 🔹 17. What is Vertical Scaling?

Increasing CPU/RAM of single instance.

Less preferred in microservices.

---

## 🔹 18. What is Observability?

Monitoring system health.

Includes:

* Logging
* Metrics
* Tracing

Tools:

* Prometheus
* Grafana

---

## 🔹 19. What is Distributed Tracing?

Tracking request across multiple services.

Example:

* Jaeger

---

## 🔹 20. What are common challenges in Microservices?

* Network latency
* Data consistency
* Debugging complexity
* Deployment complexity

---

## 🔹 21. What is CAP Theorem?

A distributed system can only guarantee two:

* Consistency
* Availability
* Partition tolerance

In microservices:
Partition tolerance is mandatory.

---

## 🔹 22. What is Bulkhead Pattern?

Isolates failures.

Example:
Separate thread pools per service call.

Prevents full system crash.

---

## 🔹 23. What is Blue-Green Deployment?

Two identical environments:

* Blue (current)
* Green (new)

Switch traffic after testing.

---

## 🔹 24. What is Canary Deployment?

Release to small % of users first.

Monitor → then full rollout.

---

## 🔹 25. Why Microservices over Monolith?

Because:

* Faster independent releases
* Better scalability
* Technology flexibility (.NET, Java, Python together)
* Fault isolation
* Cloud-native friendly

 

# 🎓 Evaluation Tip for You

When students answer:

Good answer → Concept clarity
Better answer → Real-world example
Best answer → Failure scenario explanation

 

# 🧠 Mentor Closing Thought

If a student understands:

* Saga
* Circuit breaker
* Service discovery
* Container orchestration
* Observability

Then they are no longer just developers…

They are thinking like **Solution Architects**.

 