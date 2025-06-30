
## **Rolling Deployments with Kubernetes & AWS ECS**

*"We’ve gone from coding to containerizing to automating with CI/CD. Now let’s put your app in a real orchestrated environment so you can deploy, scale, and update like a pro — without downtime."*

## 🚀 Option 1: Kubernetes Rolling Deployment

### 🌐 What is Kubernetes?

Kubernetes (K8s) is a container orchestrator that **schedules, scales, and heals your containers automatically**. It can run anywhere: your laptop (Minikube/k3d), on-prem, or in the cloud (EKS, AKS, GKE).

### 🗺️ Architecture for Our App

* **Deployment** for Node.js app
* **Service** to expose the app (LoadBalancer or NodePort)
* **Deployment** for MySQL (but best practice: use a managed database like RDS; here we’ll show basic MySQL for local dev)

### 📁 Project Structure

```
k8s/
  app-deployment.yaml
  app-service.yaml
  db-deployment.yaml
  db-service.yaml
```

### 📄 app-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: node-app
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-app
        image: yourdockerhubusername/my-node-app:latest
        ports:
        - containerPort: 3000
        env:
        - name: DB_HOST
          value: mysql
        - name: DB_USER
          value: root
        - name: DB_PASSWORD
          value: root
        - name: DB_NAME
          value: testdb
```

### 📄 app-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-app-service
spec:
  type: LoadBalancer
  selector:
    app: node-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
```

### 📄 db-deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:8
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
        - name: MYSQL_DATABASE
          value: testdb
        ports:
        - containerPort: 3306
```

### 📄 db-service.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  selector:
    app: mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
```


### 🏗️ Deploy to Kubernetes

1️⃣ Create a cluster (e.g., Minikube or EKS):

```bash
minikube start
```

2️⃣ Apply manifests:

```bash
kubectl apply -f k8s/db-deployment.yaml
kubectl apply -f k8s/db-service.yaml
kubectl apply -f k8s/app-deployment.yaml
kubectl apply -f k8s/app-service.yaml
```

3️⃣ Get your service IP:

```bash
kubectl get svc node-app-service
```

4️⃣ Access your app at `http://<external-ip>`.


### ✅ Rolling Updates with Kubernetes

If you update your image tag in your Deployment manifest:

```yaml
image: yourdockerhubusername/my-node-app:v2
```

Then run:

```bash
kubectl apply -f k8s/app-deployment.yaml
```

Kubernetes **automatically rolls out new pods**, waits for them to become healthy, and then terminates old ones — **zero downtime deployment**!

## ⚙️ Option 2: AWS ECS Rolling Deployment

### 🛰️ What is AWS ECS?

ECS (Elastic Container Service) is AWS’s managed container orchestrator. It’s simpler than Kubernetes for many use cases and integrates beautifully with AWS networking, IAM, CloudWatch, etc.

### 📋 Key Components in ECS

✅ **Task Definition** – blueprint describing your container(s)
✅ **Service** – manages running copies of your task definition
✅ **Cluster** – group of resources (EC2 or Fargate) to run your services
✅ **Load Balancer** – for zero-downtime routing

### 🛠️ Steps for Rolling Deployment on ECS

1️⃣ **Push Docker Image**

* Your CI pipeline pushes image to Amazon ECR or Docker Hub.

2️⃣ **Create Task Definition**

* Define your app container with environment vars:

```json
{
  "family": "node-app",
  "containerDefinitions": [
    {
      "name": "node-app",
      "image": "yourdockerhubusername/my-node-app:latest",
      "portMappings": [
        {
          "containerPort": 3000
        }
      ],
      "environment": [
        {"name": "DB_HOST", "value": "your-rds-endpoint"},
        {"name": "DB_USER", "value": "root"},
        {"name": "DB_PASSWORD", "value": "root"},
        {"name": "DB_NAME", "value": "testdb"}
      ]
    }
  ]
}
```

3️⃣ **Create ECS Service**

* Use AWS Console or CLI to create a Service with desired count (e.g., 2 tasks), attach to a load balancer target group.

4️⃣ **Deploy New Version**

* Register new Task Definition revision with updated image (e.g., `:v2`).
* Update ECS Service to use new revision:

```bash
aws ecs update-service \
  --cluster your-cluster-name \
  --service node-app-service \
  --task-definition node-app:v2
```

ECS will **gradually replace old tasks with new ones**, monitoring health during rollout.

### ✅ Rolling Updates with ECS

* ECS will **launch new tasks**, wait for them to pass health checks, and **deregister old tasks** from the load balancer — this gives you a **rolling, zero-downtime deployment** out-of-the-box.

## 🚨 Pro Tip: Use Managed Database

Running MySQL inside Kubernetes/ECS is fine for dev, but in production, use a managed database like **AWS RDS** for durability, backups, and scaling. Then point your app’s `DB_HOST` to the RDS endpoint.

## 🏁 **You Did It!**

✅ Your Node.js app now runs in **Kubernetes or AWS ECS**, scaling up/down as needed.
✅ Rolling deployments let you deploy updates without downtime.
✅ Combined with CI/CD, you have a complete, modern production pipeline!


👨‍🏫 *Remember*: Orchestration platforms like K8s & ECS aren’t just deployment tools — they’re powerful ecosystems that handle scaling, health checks, auto-recovery, and more.
