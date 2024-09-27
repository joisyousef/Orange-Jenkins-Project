pipeline {
    agent any

    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/joisyousef/Orange-Jenkins'
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                script {
                    sh 'docker build -t joisyousef/backend:latest -f Dockerfile.backend .'
                }
            }
        }
        stage('Build Proxy Docker Image') {
            steps {
                script {
                    sh 'docker build -t joisyousef/proxy:latest -f Dockerfile.proxy .'
                }
            }
        }
        stage('Push Docker Images') {
            steps {
                script {
                    sh 'docker push joisyousef/backend:latest'
                    sh 'docker push joisyousef/proxy:latest'
                }
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    sh 'kubectl apply -f Deployments/Backend-Deployment.yaml'
                    sh 'kubectl apply -f Deployments/Proxy-Deployment.yaml'
                    sh 'kubectl apply -f Deployments/Database-Deployment.yaml'
                    sh 'kubectl apply -f Services/Backend-Service.yaml'
                    sh 'kubectl apply -f Services/Nodeport.yaml'
                    sh 'kubectl apply -f Services/Database-Service.yaml'
                    sh 'kubectl apply -f Volumes/Database-pv.yaml'
                    sh 'kubectl apply -f Volumes/Database-pvc.yaml'
                    sh 'kubectl apply -f Volumes/Database-Secret.yaml'
                }
            }
        }
    }
}
