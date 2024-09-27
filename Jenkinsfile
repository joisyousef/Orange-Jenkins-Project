pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/joisyousef/Orange-Jenkins-project.git'
            }
        }
        stage('List Workspace') {
            steps {
                sh 'ls -la' // List files to check Dockerfile locations
            }
        }
        stage('Build Backend Docker Image') {
            steps {
                script {
                    sh 'docker build -t joisyousef/backend:latest -f Deployments/Dockerfile.backend .'
                }
            }
        }
        stage('Build Proxy Docker Image') {
            steps {
                script {
                    sh 'docker build -t joisyousef/proxy:latest -f Deployments/Dockerfile.proxy .'
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
                    sh '''
                    kubectl apply -f Deployments/Backend-Deployment.yaml
                    kubectl apply -f Deployments/Proxy-Deployment.yaml
                    kubectl apply -f Deployments/Database-Deployment.yaml
                    kubectl apply -f Services/Backend-Service.yaml
                    kubectl apply -f Services/Nodeport.yaml
                    kubectl apply -f Services/Database-Service.yaml
                    kubectl apply -f Volumes/Database-pv.yaml
                    kubectl apply -f Volumes/Database-pvc.yaml
                    kubectl apply -f Volumes/Database-Secret.yaml
                    '''
                }
            }
        }
    }
}
