// This Jenkinsfile defines a Declarative Pipeline for a typical CI/CD workflow.
// It's designed to be stored at the root of your Git repository.

pipeline {
    // --- Agent Configuration ---
    // Defines where the pipeline will run.
    // 'any' means Jenkins will pick any available agent.
    // For more control, use a specific label (e.g., 'linux-builder', 'docker').
    // Example: agent { label 'my-custom-agent' }
    // Example with Docker: agent { docker { image 'maven:3.8.7-openjdk-17' } }
    agent any

    // --- Environment Variables ---
    // Define global environment variables for the pipeline.
    // These can be used across all stages.
    environment {
        // Example: Application name for consistent naming
        APP_NAME = 'my-web-app'
        // Example: Docker registry URL (if you're building/pushing images)
        // DOCKER_REGISTRY = 'your-docker-registry.com'
        // Example: Kubernetes namespace for deployment
        // KUBE_NAMESPACE = 'dev'
    }

    // --- Stages Definition ---
    // Divides the pipeline into logical, sequential steps.
    stages {
        // --- Stage 1: Checkout Source Code ---
        // Retrieves the code from the Git repository where this Jenkinsfile resides.
        stage('Checkout Code') {
            steps {
                // 'checkout scm' is a special step that checks out the SCM configured for the job.
                // When using "Pipeline script from SCM", this automatically clones the repo.
                checkout scm
                echo "Source code checked out for ${env.APP_NAME}."
            }
        }

        // --- Stage 2: Build Application ---
        // Compiles the code, runs unit tests, and potentially packages the application.
        // This stage will vary greatly based on your project's technology (Maven, npm, Go, etc.).
        stage('Build') {
            steps {
                script {
                    // Example for a Java Maven project:
                    // sh 'mvn clean install -DskipTests'

                    // Example for a Node.js project:
                    // sh 'npm install'
                    // sh 'npm run build'

                    // Example for a simple Go project:
                    // sh 'go build -o myapp .'

                    // Placeholder for now
                    echo "Building ${env.APP_NAME}..."
                    // Create a dummy artifact for demonstration
                    sh 'echo "This is a build artifact for ${APP_NAME}" > build/${APP_NAME}.jar'
                }
            }
        }

        // --- Stage 3: Run Unit Tests (Optional but Recommended) ---
        // Executes automated unit tests to ensure code quality.
        stage('Unit Tests') {
            steps {
                script {
                    // Example for a Java Maven project:
                    // sh 'mvn test'

                    // Example for a Node.js project:
                    // sh 'npm test'

                    echo "Running unit tests for ${env.APP_NAME} (simulated)."
                    // Simulate test success
                    sh 'echo "All unit tests passed!"'
                }
            }
        }

        // --- Stage 4: Build Docker Image (Optional for Containerized Apps) ---
        // If your application is containerized, this stage builds the Docker image.
        // Requires Docker to be installed on the Jenkins agent or accessible via Docker-in-Docker.
        // Also often requires credential setup in Jenkins for pushing to private registries.
        stage('Build Docker Image') {
            when {
                expression {
                    // Only run this stage if a Dockerfile exists in the workspace
                    fileExists 'Dockerfile'
                }
            }
            steps {
                script {
                    // Example:
                    // withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    //     sh "docker build -t ${env.DOCKER_REGISTRY}/${env.APP_NAME}:${env.BUILD_NUMBER} ."
                    //     sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${env.DOCKER_REGISTRY}"
                    //     sh "docker push ${env.DOCKER_REGISTRY}/${env.APP_NAME}:${env.BUILD_NUMBER}"
                    // }
                    echo "Building Docker image for ${env.APP_NAME}:${env.BUILD_NUMBER} (simulated)."
                    sh 'echo "FROM scratch\nCOPY . /app" > Dockerfile' // Create a dummy Dockerfile
                    sh 'echo "Dummy Docker image built."'
                }
            }
        }

        // --- Stage 5: Deploy to Kubernetes/Cloud (Optional) ---
        // Deploys the application to a target environment (e.g., Kubernetes, Cloud Provider).
        // Requires kubectl/oc on the agent, and credentials configured in Jenkins.
        stage('Deploy to Dev') {
            steps {
                script {
                    // Example for Kubernetes deployment:
                    // Assuming your Kubernetes YAML files are in a 'k8s' directory
                    // sh "kubectl apply -f k8s/deployment.yaml -n ${env.KUBE_NAMESPACE}"
                    // sh "kubectl apply -f k8s/service.yaml -n ${env.KUBE_NAMESPACE}"

                    // Recommended: Use Jenkins Kubernetes CLI plugin for secure kubeconfig usage
                    // withKubeConfig(credentialsId: 'your-kubeconfig-credential-id') {
                    //     sh "kubectl apply -f k8s/deployment.yaml -n ${env.KUBE_NAMESPACE}"
                    //     sh "kubectl apply -f k8s/service.yaml -n ${env.KUBE_NAMESPACE}"
                    // }

                    echo "Deploying ${env.APP_NAME} to Dev environment (simulated)."
                    sh 'echo "Dummy Kubernetes deployment applied."'
                }
            }
        }

        // --- Stage 6: Run Integration/End-to-End Tests (Optional) ---
        // After deployment, run tests against the deployed application.
        stage('Integration Tests') {
            when {
                // Only run this stage if the 'Deploy' stage completed successfully
                expression { currentBuild.currentResult == null || currentBuild.currentResult == 'SUCCESS' }
            }
            steps {
                echo "Running integration tests for ${env.APP_NAME} (simulated)."
                // Example: Trigger Cypress/Selenium tests, or API tests
                // sh 'npm run e2e-tests'
                sh 'echo "All integration tests passed!"'
            }
        }
    }

    // --- Post-build Actions ---
    // Actions that run after all stages are completed, regardless of success or failure.
    post {
        always {
            echo "Pipeline finished for ${env.APP_NAME}."
            // Clean up the workspace to save disk space
            cleanWs()
        }
        success {
            echo "Pipeline completed successfully! ✅"
            // Example: Send Slack notification
            // slackSend channel: '#devops', message: "Deployment of ${env.APP_NAME} (Build ${env.BUILD_NUMBER}) succeeded!"
        }
        failure {
            echo "Pipeline failed! ❌"
            // Example: Send email notification on failure
            // mail to: 'devops-team@example.com',
            //      subject: "Jenkins Build Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            //      body: "Check console output for details: ${env.BUILD_URL}"
        }
        unstable {
            echo "Pipeline completed with some issues (unstable)."
        }
        aborted {
            echo "Pipeline was aborted."
        }
    }
}
