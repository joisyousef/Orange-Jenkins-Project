pipeline {
    agent any
    stages {
          stage('CI') {
            steps {
                // Clone your repository
                git 'https://github.com/joisyousef/Orange-Jenkins-Project.git'

                // Build and push the Docker image using credentials for Docker Hub
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    docker login -u ${USERNAME} -p ${PASSWORD}
                    docker build . -t ${USERNAME}/backend:latest --network host
                    docker push ${USERNAME}/backend:latest
                    """
                }
            }
        }
        stage('CD') {
            steps {
                // Clone your repository to get the YAML files
                git 'https://github.com/joisyousef/Orange-Jenkins-Project.git'

                // Deploy the application using kubectl and apply the deployment and service YAML files
                withCredentials([usernamePassword(credentialsId: 'docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
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
    }


