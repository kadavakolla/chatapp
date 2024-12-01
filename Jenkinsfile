pipeline {
    agent any
    environment {
       FRONTEND_URL='http://localhost:3000'
       MONGODB_URI='mongodb+srv://asrithnune03:asrithrishi@discuss.lieci.mongodb.net/?retryWrites=true&w=majority&appName=DiscUss'
       JWT_SECRET_KEY='jhdcjhsdvchjsdhbfasdgbvs'
    }
    stages {
        stage('Clone Git') {
            steps {
                git branch: 'main', url: 'https://github.com/kadavakolla/chatapp.git'
            }
        }
        stage('Build Frontend Image') {
            steps {
                dir('client'){
                sh "npm install"
                sh 'docker build -t frontend-image .'
            }
            }
        }
        stage('Build Backend Image') {
            steps {
                dir('server'){
                sh "npm install"
                sh 'docker build -t backend-image .'
            }
            }
        }
        stage('Push Images to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCred', usernameVariable: 'asrith1158', passwordVariable: 'Chandrausha@123')]) {
                    sh 'docker login -u $asrith1158 -p $Chandrausha@123'
                    sh 'docker tag frontend-image asrith1158/frontend-image:latest'
                    sh 'docker push asrith1158/frontend-image:latest'
                    sh 'docker tag backend-image asrith1158/backend-image:latest'
                    sh 'docker push asrith1158/backend-image:latest'
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'ansible-galaxy collection install kubernetes.core'
                }
            }
        }
        stage('Ansible Deployment') {
            steps {
                script {
                    sh 'ansible-playbook -i inventory-k8 playbook-k8.yml'
                }
            }
        }
    }
}
