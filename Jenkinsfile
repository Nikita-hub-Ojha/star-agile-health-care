pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Nikita-hub-Ojha/star-agile-health-care.git'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t nikitaojha/staragileprojecthealthcare:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push nikitaojha/staragileprojecthealthcare:v1'
            }
        }  
        stage('Deploy using K8s') {
            steps {
                sh  'kubectl apply -f deployment/kubernetesfile.yml'
                sh 'kubectl get all'
                }
        }  
                 
    }
}    
