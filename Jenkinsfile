pipeline {
    agent any
    stages {
        stage('Checkout Source') {
            steps {
                git branch: 'main', url: 'https://github.com/halimo22/Jenkins_project.git'
            }
        }
        stage('Push Backend') {
            steps {
                build job: 'backend-publish'
            }
        }
        stage('Push Proxy') {
            steps {
                build job: 'proxy-publish'
            }
        }
        stage('Deploying to Kubernetes') {
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
                        kubectl apply -f Volumes/Databasepvc.yaml
                        kubectl apply -f Volumes/Database-Secret.yaml
                    '''
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
