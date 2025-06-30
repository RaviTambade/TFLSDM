Fantastic — you’re thinking like a pro now! Let’s build on your Dockerized Node.js + MySQL app by setting up a **CI/CD pipeline** so every time you push code, your containers get rebuilt, tested, and deployed automatically.

We’ll do it with **GitHub + Jenkins**, but I’ll also give you a note on GitHub Actions if you prefer a simpler route.

---

## 👨‍🏫 Mentor Storytelling: **Automating Your Docker App Deployment with CI/CD**

---

*"Remember when you learned to containerize your app? That solved ‘works on my machine.’ Now let’s solve ‘deployed manually every time.’ With CI/CD, you commit code → pipeline runs → containers are built → app is deployed. Hands off, reliable, fast — this is modern DevOps."*

---

## 🛠️ **High-Level Plan**

✅ Set up Jenkins to:

1. Pull latest code from GitHub
2. Build Docker images for `app`
3. Push images to a registry (e.g., Docker Hub)
4. Deploy updated containers to your server (e.g., AWS EC2)

---

## 🔗 **Step 1: Prepare Docker Hub**

* Create an account on [Docker Hub](https://hub.docker.com).
* Create a repo like `yourdockerhubusername/my-node-app`.
* Generate Docker Hub credentials (username & password) — you’ll add them to Jenkins later.

---

## 🔧 **Step 2: Install Jenkins**

1. Launch Jenkins on your local machine, a cloud VM, or AWS EC2 (install instructions [here](https://www.jenkins.io/doc/book/installing/)).
2. Open Jenkins at `http://<jenkins-ip>:8080`.
3. Install recommended plugins + extra:

   * Docker Pipeline Plugin
   * Git Plugin

---

## 🗝️ **Step 3: Add Credentials**

* In Jenkins → **Manage Jenkins → Credentials → System → Global credentials → Add Credentials**:

  * **Kind**: Username with password
  * **Username**: Docker Hub username
  * **Password**: Docker Hub password or token
  * **ID**: `docker-hub-creds`

---

## 🚦 **Step 4: Create a Jenkins Pipeline Job**

* Go to Jenkins dashboard → **New Item → Pipeline → your-project-name → OK**.
* In the job config, set **Pipeline script** to use a `Jenkinsfile` from your GitHub repo.

---

## 📜 **Step 5: Add a Jenkinsfile**

Add a file named `Jenkinsfile` to your repo:

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
                // Simplest: just print a success message.
                // Advanced: SSH to server + docker-compose pull + restart containers
                echo "Image pushed: ${DOCKER_IMAGE} - deploy manually or automate with ssh if ready."
            }
        }
    }
}
```

---

## 🚢 **Step 6: Automate Deployment**

To **fully automate deployment**, your Jenkins pipeline can SSH into your server (e.g., EC2) to pull the new image & restart containers.

Example `Deploy` stage:

```groovy
stage('Deploy') {
    steps {
        sshagent(credentials: ['my-ssh-key']) {
            sh """
                ssh -o StrictHostKeyChecking=no ec2-user@your-ec2-ip '
                    cd /path/to/your/docker-compose/folder &&
                    docker-compose pull app &&
                    docker-compose up -d'
            """
        }
    }
}
```

🔹 **my-ssh-key**: Add your server’s SSH key to Jenkins credentials.

🔹 In your `docker-compose.yml`, change the app service to:

```yaml
app:
  image: yourdockerhubusername/my-node-app:latest
  ...
```

Then on your server:

```bash
docker-compose pull app
docker-compose up -d
```

---

## 📝 **How This Pipeline Works**

1️⃣ **Every push → Jenkins runs pipeline.**
2️⃣ **Build** a Docker image from latest code.
3️⃣ **Push** the image to Docker Hub with a unique tag (`BUILD_NUMBER`).
4️⃣ **Deploy**: Pulls latest image on the server & restarts containers.

---

## 🛡️ **Benefits**

✅ Fully automated — commit code → live in production
✅ Traceable — each build has a unique Docker image tag
✅ Rollback-friendly — just redeploy an earlier image
✅ Consistent environments from dev to prod

---

## ⚡ **Alternative: GitHub Actions**

If you don’t want Jenkins, you can do this entirely in GitHub with a **GitHub Actions workflow**. Here’s a minimal example:

**.github/workflows/docker.yml**

```yaml
name: Build & Deploy Docker

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v4
    - name: Log in to Docker Hub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}
    - name: Build Docker image
      run: docker build -t yourdockerhubusername/my-node-app:${{ github.run_number }} .
    - name: Push Docker image
      run: docker push yourdockerhubusername/my-node-app:${{ github.run_number }}
```

---

## 🎉 **You Did It!**

You now have a **complete CI/CD pipeline**:
✅ Automates Docker builds
✅ Pushes images to Docker Hub
✅ Deploys updated containers
✅ Reduces manual steps to almost zero

---

👨‍🏫 *Remember*: CI/CD + containers = modern DevOps. Small steps like this build your mastery.

Would you like help adding automated tests or rolling deployments?
