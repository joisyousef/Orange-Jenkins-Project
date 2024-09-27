pipeline {
    agent any

    stages {
        stage('CD') {
            steps {
                // Clone your repository to get the YAML files
                withCredentials([usernamePassword(credentialsId: 'your-credentials-id', usernameVariable: 'GITHUB_USERNAME', passwordVariable: 'GITHUB_TOKEN')]) {
                    sh 'git clone https://${GITHUB_USERNAME}:${GITHUB_TOKEN}@github.com/joisyousef/Orange-Jenkins-Project.git'
                }

                // Deploy the application using kubectl and apply the deployment and service YAML files
                sh """
                kubectl set image deployment/backend-deployment backend=joisyousef/backend:latest --namespace app
                kubectl apply -f /var/jenkins_home/workspace/Orange-Jenkins-Project/Deployments/Backend-Deployment.yaml
                kubectl apply -f /var/jenkins_home/workspace/Orange-Jenkins-Project/Services/Backend-Service.yaml
                kubectl apply -f /var/jenkins_home/workspace/Orange-Jenkins-Project/Services/Nodeport.yaml
                """
            }
        }
    }
}
