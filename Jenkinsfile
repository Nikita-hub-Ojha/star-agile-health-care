pipeline {
    agent any

     environment {
         DOCKER_IMAGE = 'nikitaojha/staragileprojecthealthcare:v1'
         EKS_CLUSTER_NAME = 'capstone-eks'
         AWS_REGION = 'us-east-1'
     }

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
        stage('Deploy to k8sr') {
            steps {
                script {
                    sh '''
                    echo "Verifying AWS credentials..."
                    aws sts get-caller-identity

                    echo "Configuring kubectl for EKS cluster..."
                    aws eks update-kubeconfig --name $EKS_CLUSTER_NAME --region $AWS_REGION

                    echo "Verifying kubeconfig..."
                    kubectl config view

                    echo "Deploying application to EKS..."
                    kubectl apply -f kubernetesfile.yml
                    

                    echo "Verifying deployment..."
                    kubectl get pods
                    kubectl get svc
                    '''
                }
            }
        }
          


           
           
    
                  
    }
}    
         
