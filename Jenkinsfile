pipeline {
    agent { label 'dev-agent' }
    stages {
        stage('Code'){
            steps{
                git url:'https://github.com/lingarajtechhub/node-todo-cicd.git', branch:'master'
            }
        }
        stage('Build & Test'){
            steps{
                sh "docker build . -t node-todo-app"
            }
        }
        stage('Login & Push Image To Docker Hub'){
            steps{
                echo "login to docker hub and push image..."
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker tag node-todo-app ${env.dockerHubUser}/node-todo-app:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/node-todo-app:latest"
                }
            }
        }
        stage('Deploy'){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
    
}
