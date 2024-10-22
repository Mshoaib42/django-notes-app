@Library("Shared") _
pipeline {
    agent any
    
    stages {
        stage("Code") {
            steps {
                echo "Checking out code"
                git url: "https://github.com/Abdullah-dev-max/my-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the project"
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Tagging and pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerHub", passwordVariable: "dockerHubPass", usernameVariable: "dockerHubUser")]) {
                    // Tag the Docker image with the Docker Hub username
                    sh "docker tag my-note-app ${dockerHubUser}/my-note-app:latest"
                    
                    // Login to Docker Hub
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    
                    // Push the Docker image to Docker Hub
                    sh "docker push ${dockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                // Use the correct image name that you pushed to Docker Hub
                //sh "docker run -d -p 8000:8000 ahmadhassan749/my-note-app:latest"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
