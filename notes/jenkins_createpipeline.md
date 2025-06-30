# Creating a pipeline job in Jenkins

Creating a pipeline job in Jenkins involves defining your build process using a Jenkinsfile, which is a text file that contains the definition of your Jenkins pipeline. Let us create and configure a pipeline job in Jenkins:

### Prerequisites

1. **Jenkins Installation**: Ensure Jenkins is installed and running on your server or within a Docker container as per the previous instructions.

2. **Access Jenkins Dashboard**: Access Jenkins through your web browser using `http://your-ec2-public-ip:8080` (replace with your EC2 instance's public IP or domain).

### Steps to Create a Pipeline Job

#### 1. **Create a New Pipeline Job**

- Go to Jenkins dashboard.
- Click on `New Item` on the left sidebar.
- Enter a name for your job (e.g., `MyPipelineJob`).
- Select `Pipeline` and click `OK`.

#### 2. **Configure Pipeline Job**

- **General Tab**:
  - Enter a description (optional).
  
- **Pipeline Tab**:
  - **Definition**: Select `Pipeline script from SCM`.
  - **SCM**: Choose your Version Control System (e.g., Git).
  - **Repository URL**: Provide the URL of your Git repository where your Jenkinsfile is located.
  - **Script Path**: Specify the path to your Jenkinsfile (e.g., `Jenkinsfile` if it's in the root directory).

#### 3. **Create Jenkinsfile**

Create a Jenkinsfile in your Git repository. This file defines the stages and steps of your pipeline job. Here’s a basic example:

```groovy
pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                // Example: Run a shell command
                sh 'echo "Building..."'
            }
        }
        stage('Test') {
            steps {
                // Example: Run unit tests
                sh 'echo "Testing..."'
            }
        }
        stage('Deploy') {
            steps {
                // Example: Deploy to a server
                sh 'echo "Deploying..."'
            }
        }
    }
}
```

Replace the `sh` steps with actual commands relevant to your build, test, and deployment process.

#### 4. **Save and Commit Jenkinsfile**

Save the Jenkinsfile in your Git repository and commit your changes.

#### 5. **Run the Pipeline Job**

- Go back to Jenkins dashboard.
- Click on your pipeline job (`MyPipelineJob`).
- Click `Build Now` to run the pipeline job.

#### 6. **View Pipeline Execution**

- Jenkins will execute the pipeline according to the stages and steps defined in your Jenkinsfile.
- View the progress of each stage in the Jenkins UI.
- Console output logs can provide detailed information about each step’s execution.

### Additional Considerations

- **Parameters**: You can add parameters to your pipeline job to make it more flexible.
- **Error Handling**: Implement error handling in your Jenkinsfile to manage failures gracefully.
- **Notifications**: Configure email notifications or integrate with messaging services to notify on build status changes.

By following these steps, you can successfully create and configure a pipeline job in Jenkins using a Jenkinsfile. This approach provides a structured and repeatable way to define and execute your build, test, and deployment processes within Jenkins. Adjust the Jenkinsfile according to your project’s specific requirements and workflows.


## Setting up a Jenkins pipeline for a Node.js Express web application 

Setting up a Jenkins pipeline for a Node.js Express web application involves configuring Jenkins to build, test, and possibly deploy your application using a Jenkinsfile. Below are the detailed steps to set up this pipeline:

### Prerequisites

1. **Jenkins Installation**: Ensure Jenkins is installed and running on your server or within a Docker container.

2. **Access Jenkins Dashboard**: Access Jenkins through your web browser using `http://your-ec2-public-ip:8080` (replace with your Jenkins server's URL).

3. **Node.js and npm**: Install Node.js and npm on the Jenkins server or the build agent where Jenkins will execute your pipeline.

4. **Git**: Ensure Git is installed on the Jenkins server or the build agent.

### Steps to Set Up Jenkins Pipeline for Node.js Express Web App

#### 1. **Create a New Pipeline Job in Jenkins**

- Go to Jenkins dashboard.
- Click on `New Item` on the left sidebar.
- Enter a name for your job (e.g., `NodeExpressAppPipeline`).
- Select `Pipeline` and click `OK`.

#### 2. **Configure Pipeline Job**

- **General Tab**:
  - Enter a description (optional).

- **Pipeline Tab**:
  - **Definition**: Select `Pipeline script from SCM`.
  - **SCM**: Choose your Version Control System (e.g., Git).
  - **Repository URL**: Provide the URL of your Git repository where your Node.js Express web application source code is hosted.
  - **Script Path**: Specify the path to your Jenkinsfile in the repository (e.g., `Jenkinsfile`).

#### 3. **Create Jenkinsfile**

Create a `Jenkinsfile` in the root directory of your Node.js Express web application repository. The Jenkinsfile defines the stages and steps of your pipeline job. Here's an example `Jenkinsfile` for building, testing, and deploying a Node.js Express web application:

```groovy
pipeline {
    agent any
    
    environment {
        NODE_VERSION = '14' // Specify your Node.js version
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout source code from Git repository
                git branch: 'main', url: 'https://github.com/yourusername/your-repo.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                // Install Node.js dependencies
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run tests (replace with your test command)
                sh 'npm test'
            }
        }
        
        stage('Build and Deploy') {
            steps {
                // Build and deploy your application (replace with your build and deploy commands)
                sh 'npm run build'
                sh 'npm run deploy'
            }
        }
    }
}
```

Replace the `sh` steps with actual commands relevant to your build, test, and deployment process.

#### 4. **Save and Commit Jenkinsfile**

Save the `Jenkinsfile` in your Git repository and commit your changes.

#### 5. **Run the Pipeline Job**

- Go back to Jenkins dashboard.
- Click on your pipeline job (`NodeExpressAppPipeline`).
- Click `Build Now` to run the pipeline job.

#### 6. **View Pipeline Execution**

- Jenkins will execute the pipeline according to the stages and steps defined in your Jenkinsfile.
- View the progress of each stage in the Jenkins UI.
- Console output logs can provide detailed information about each step’s execution.

### Additional Considerations

- **Parameters**: You can add parameters to your pipeline job to make it more flexible (e.g., for different environments or configurations).
- **Error Handling**: Implement error handling in your Jenkinsfile to manage failures gracefully.
- **Security**: Ensure proper security measures are in place, especially for deployment steps.

### Summary

By following these steps, you can set up a Jenkins pipeline to build, test, and deploy a Node.js Express web application. The pipeline automates the build, test, and deployment process, providing consistency and reliability in your application delivery workflow. Customize the Jenkinsfile according to your project’s specific requirements and deployment targets. Adjust the Node.js version and npm commands as per your application's needs.


```
pipeline {
 agent any
 environment {
  dotnet = 'C:\\Program Files\\dotnet\\dotnet.exe'
 }
 stages {
  stage('Checkout') {
   steps {
    git credentialsId: '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', url: 'https://github.com/RaviTambade/TFLECommerce.git', branch: 'master'
   }
  }
  
  stage('Build') {
   steps {
    bat 'dotnet build WebSolution\\ECommerceAPI\\ECommerceAPI.csproj --configuration Release'
   }
  }
  stage('Test') {
   steps {
    bat 'dotnet test JenkinsIntegration\\JenkinsIntegration.Tests\\JenkinsIntegration.Tests.csproj --logger:trx'
   }
  }
 }
}
```