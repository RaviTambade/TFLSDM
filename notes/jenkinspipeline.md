# Simple Jenkins pipeline using configuration

To build a simple Jenkins pipeline using configuration, follow these step-by-step instructions:

### Step 1: Install Jenkins
1. **Install Jenkins** on your system or use a cloud instance to set up Jenkins. Follow the official installation guides for your operating system: [Jenkins Installation](https://www.jenkins.io/doc/book/installing/).

2. Once installed, open Jenkins in your web browser by going to `http://localhost:8080` (or the URL provided in the installation instructions).

3. **Unlock Jenkins** by entering the administrator password found in the installation logs or specified location on your machine.

### Step 2: Create a New Jenkins Job (Pipeline)

1. After logging into Jenkins, click on the **"New Item"** link from the Jenkins dashboard.
2. In the "Enter an item name" field, enter a name for your pipeline (e.g., "MyFirstPipeline").
3. Select **"Pipeline"** as the project type.
4. Click the **OK** button.

### Step 3: Configure Pipeline Script (Declarative Pipeline)

1. On the configuration page, scroll down to the **Pipeline** section.
2. Choose the **Pipeline Script** option to write your Jenkins pipeline directly in the configuration.
3. In the **Pipeline Script** text area, you can define your pipeline using a Declarative Pipeline syntax. Here's a simple example:

   ```groovy
   pipeline {
       agent any  // Run on any available agent

       stages {
           stage('Checkout') {
               steps {
                   // Checkout code from Git repository
                   git 'https://github.com/your-username/your-repository.git'
               }
           }
           stage('Build') {
               steps {
                   // Run build commands (e.g., Maven, Gradle, npm, etc.)
                   sh 'echo "Building the project"'
               }
           }
           stage('Test') {
               steps {
                   // Run tests (e.g., unit tests, integration tests)
                   sh 'echo "Running tests"'
               }
           }
           stage('Deploy') {
               steps {
                   // Deploy application (e.g., copy files, deploy to server)
                   sh 'echo "Deploying application"'
               }
           }
       }
   }
   ```

4. After entering the script, click **Save** to store your pipeline configuration.

### Step 4: Add Pipeline to Version Control (Optional)

If your pipeline is part of your source code repository, you can define the pipeline in a `Jenkinsfile` in your repository. The pipeline would look like this:

1. Create a file named `Jenkinsfile` in your repository root.
2. Add your pipeline script to the file (similar to the one shown above).
3. Push the file to your repository.
4. In Jenkins, configure the job to pull the pipeline script from the repository instead of defining it manually.

   - Go back to the pipeline job configuration page.
   - Under **Pipeline**, select **"Pipeline script from SCM"**.
   - Choose the appropriate source control management (e.g., Git).
   - Enter the repository URL and branch information.

### Step 5: Execute the Pipeline

1. After saving your pipeline configuration, you’ll be redirected to the job page.
2. Click **"Build Now"** to run the pipeline.
3. Monitor the pipeline execution in the **Build History** section and view logs by clicking on the build number.

### Step 6: Monitor and Review Results

1. After the pipeline run completes, click on the build number to view the details.
2. Review the logs and output of each stage to ensure the pipeline executed successfully.
3. If any issues occur, the error logs will guide you to identify and fix them.

### Step 7: Automate Triggers (Optional)

To trigger the pipeline automatically, you can configure triggers such as:

- **GitHub webhook**: Automatically trigger the pipeline on code changes (push, PR, etc.).
- **Cron**: Schedule periodic execution (e.g., run nightly).

To configure this:

1. Go to the pipeline job configuration page.
2. Scroll to the **Build Triggers** section.
3. Enable **Poll SCM** or **GitHub hook trigger for GITScm polling** depending on your use case.

### Conclusion

You’ve now successfully created and executed a simple Jenkins pipeline using a configuration. You can expand the pipeline by adding more stages, integrating with other tools (e.g., Docker, Kubernetes, deployment tools), and implementing more advanced Jenkins features like parallel steps, notifications, and more.