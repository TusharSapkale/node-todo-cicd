pipeline {
    agent any

    stages {
        stage('GET') {
            steps {
                git url: "https://github.com/TusharSapkale/node-todo-cicd.git", branch: "master"
            }
        }
        stage('BUILD') {
            steps {
                sh "docker build -t node-app ."
            }
        }
        stage('PUSH to DOCKERHUB') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass")]){
                        
                    sh "echo ${dockerHubPass} | docker login -u ${dockerHubUser} --password-stdin"
                    sh "docker image tag node-app:latest ${dockerHubUser}/node-app:latest"
                    sh "docker push ${dockerHubUser}/node-app:latest"    
                    }
            }
        }
        stage('DEPLOY') {
            steps {
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
