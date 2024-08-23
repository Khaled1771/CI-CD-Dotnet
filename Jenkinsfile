pipeline {
    agent any

    // environment {
    //     // Docker image name and tag
    //     IMAGE_NAME = 'khaledmahmoud/asp-net'
    //     IMAGE_TAG = '7.0'
    // }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the source repository
                git branch: 'main', url: 'https://your-repo-url.git'
            }
        }

        stage('Restore Dependencies') {
            steps {
                // Restore .NET dependencies
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                // Build the .NET app
                sh 'dotnet build --configuration Release'
            }
        }

        stage('Publish') {
            steps {
                // Publish the app to prepare for deployment
                sh 'dotnet publish --configuration Release --output ./out'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Build the Docker image
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker registry (e.g., Docker Hub)
                    sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'

                    // Push the Docker image
                    sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up
            cleanWs()
        }
    }
}

