pipeline {
    agent any

    environment {
        // Docker image name and tag
        IMAGE_NAME = 'khaledmahmoud7/asp-net'
        IMAGE_TAG = '7.0'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from the source repository
                git branch: 'main', url: 'https://github.com/Khaled1771/CI-CD-Dotnet.git'
            }
        }

        // For Manual building
        // stage('Restore Dependencies') {
        //     steps {
        //         // Restore .NET dependencies
        //         sh 'dotnet restore'
        //     }
        // }

        // stage('Build') {
        //     steps {
        //         // Build the .NET app
        //         sh 'dotnet build --configuration Release'
        //     }
        // }

        // stage('Publish') {
        //     steps {
        //         // Publish the app to prepare for deployment
        //         sh 'dotnet publish --configuration Release --output ./out'
        //     }
        // }

        stage('Build Docker Image') {
            steps {
                when{
                    expression{
                        BRANCH_NAME == 'main'
                    }
                }
                // Build the Docker image
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Log in to Docker registry (e.g., Docker Hub)
                    withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                   
                        sh "echo $PASSWORD | docker login -u $USERNAME --password-stdin "
                          // Push the Docker image
                        sh "docker push ${IMAGE_NAME}:${IMAGE_TAG}"

                    }
                    
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

