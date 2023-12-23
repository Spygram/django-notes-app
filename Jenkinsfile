pipeline {
    agent any

    stages {
        stage("Code") {
            steps {
                echo 'Cloning the code'
                git url:"https://github.com/pawan8979/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo 'Building the image'
                sh "docker build -t notes-app ."
            }
        }
        stage("Push") {
            steps {
                echo 'Pushing the image to Docker Hub'
                withCredentials([usernamePassword(credentialsId:"docker-hub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                    sh "docker tag notes-app ${env.dockerHubUser}/notes-app:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/notes-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo 'Deploying the Container'
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
