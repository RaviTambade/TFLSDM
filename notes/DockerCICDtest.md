Perfect — that’s the next level of professional CI/CD! Let’s finish your pipeline strong by adding **automated tests** (so you don’t deploy broken code) and **rolling deployments** (so you update containers with zero downtime).

---

## 👨‍🏫 Mentor Storytelling: **Automated Tests + Rolling Deployments**

*"Remember when we said CI/CD is like an assembly line? Tests are like quality checks along the line, while rolling deployments let you swap parts without stopping the machine. Let’s put both into your pipeline so your app updates safely and reliably."*

---

## ✅ Part 1: Automated Tests

---

### 🧪 **Why Add Tests in CI?**

Without automated tests, you risk deploying broken code straight to production. Tests ensure your build is healthy before moving on to building images or deploying.

---

### 🛠️ **How to Add a Test Stage in Jenkins**

Update your `Jenkinsfile` to include a `Test` stage **before** building your Docker image:

```groovy
pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/my-node-app:${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/yourusername/your-repo.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                echo "Image pushed: ${DOCKER_IMAGE} - deploy manually or automate with ssh if ready."
            }
        }
    }
}
```

---

### 📝 What Happens Now?

✅ Jenkins installs dependencies.
✅ Runs your automated tests with `npm test`.
✅ If tests fail → pipeline stops → broken code never goes to Docker or production.

---

### 🚨 Pro Tip

Make sure you have a meaningful **test script** in your `package.json`:

```json
"scripts": {
  "test": "node test.js || exit 1",
  "start": "node app.js"
}
```

Where `test.js` might include:

```js
console.log("Running tests...");
// e.g., check database connection or API response
process.exit(0); // exit with success code
```

Or use a proper framework like Jest, Mocha, etc.

---

## 🔄 Part 2: Rolling Deployments

---

### 🚦 **What is Rolling Deployment?**

In rolling deployments, new containers are started before old ones are stopped. This minimizes downtime because there’s always at least one instance serving users.

---

### 🚀 **Rolling Deployment with Docker Compose**

If your app runs behind a **load balancer** (like nginx, HAProxy, or a cloud ELB), you can:

1. Launch new containers with the new image.
2. Wait for them to become healthy.
3. Remove old containers.
4. Route traffic to new containers.

---

### 🔨 **Simple Rolling with docker-compose pull & up**

On your server, run:

```bash
docker-compose pull app
docker-compose up -d --no-deps --scale app=2 app
```

Here, `--scale app=2` starts 2 containers temporarily during the rollout. You can script it in your Jenkins `Deploy` stage:

```groovy
stage('Deploy') {
    steps {
        sshagent(credentials: ['my-ssh-key']) {
            sh """
                ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-ip '
                    cd /path/to/docker-compose/folder &&
                    docker-compose pull app &&
                    docker-compose up -d --no-deps --scale app=2 app &&
                    sleep 10 &&
                    docker-compose up -d --no-deps --scale app=1 app'
            """
        }
    }
}
```

What this does:
✅ Pulls the latest image.
✅ Temporarily runs two containers → ensures new one starts & becomes healthy.
✅ Scales back to 1 container (or whatever your target is) after 10 seconds.

---

### ☁️ **Pro Tip: True Zero-Downtime with Orchestration**

For **real rolling deployments** in production with:
✅ Health checks
✅ Automatic scaling
✅ Load balancing
✅ Blue/green or canary releases

— you’d use orchestrators like **Kubernetes**, **AWS ECS**, or **Docker Swarm**.

Example with Kubernetes:

* Deploy a new ReplicaSet.
* K8s automatically rolls pods gradually.
* Monitors health and rolls back on failure.

---

## 🏁 **What You Have Now**

✅ **Automated tests** protect your pipeline.
✅ **Rolling deployments** keep your app live during updates.
✅ A professional-grade CI/CD pipeline that builds, tests, ships, and deploys your Dockerized app with minimal downtime.

---

👨‍🏫 *Remember*: The more you automate, the faster you release — and the more confidence you have in your deployments.

Would you like a hands-on guide for doing this with Kubernetes or AWS ECS?
