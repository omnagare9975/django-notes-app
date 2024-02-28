pipeline {
    agent any
    stages {
        stage("code") {
            steps {
                echo "Cloning the code"
                git url: "https://github.com/omnagare9975/django-notes-app.git", branch: "main"
            }
        }
        stage("build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-node-app ."
            }
        }
        stage("push to Docker hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubpass", usernameVariable: "dockerHubUser")]) {
                    sh "docker tag my-node-app ${env.dockerHubUser}/my-note-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerhubpass}"
                    sh "docker push ${env.dockerHubUser}/my-note-app:latest"
                }
            }
        }


        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
