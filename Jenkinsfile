pipeline {
    agent any
    environment {
        NAMESPACE = 'webapp'
        REPO_URL = 'https://github.com/joisyousef/Orange-Kubernetes-Project.git'
        DOCKER_REGISTRY = 'joisyousef'  // Your Docker Hub username
        registryCredential = 'docker-hub-credentials' // Your Jenkins credentials ID for Docker Hub
    }
    stages {
        stage('Setup Namespace') {
            steps {
                script {
                    // Create the webapp namespace if it doesn't exist
                    sh "kubectl create namespace ${NAMESPACE} || echo 'Namespace ${NAMESPACE} already exists.'"
                }
            }
        }
        stage('Checkout') {
            steps {
                // Checkout the repository from the specified URL
                git branch: 'main', url: "${REPO_URL}"
            }
        }
        stage('Build and Push Backend Image') {
            steps {
                script {
                    // Build the backend Docker image with the Jenkins build number as the tag
                    sh """
                    docker build -t ${DOCKER_REGISTRY}/backend:${env.BUILD_NUMBER} -f Dockerfile .
                    """
                    // Push the backend image after building it
                    withDockerRegistry([ credentialsId: "docker-hub-credentials", url: "" ]) {
                        sh "docker push ${DOCKER_REGISTRY}/backend:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Build and Push Nginx Image') {
            steps {
                script {
                    // Build the Nginx Docker image with the Jenkins build number as the tag
                    sh """
                    docker build -t ${DOCKER_REGISTRY}/nginx:${env.BUILD_NUMBER} -f Dockerfile.nginx .
                    """
                    // Push the Nginx image after building it
                    withDockerRegistry([ credentialsId: "docker-hub-credentials", url: "" ]) {
                        sh "docker push ${DOCKER_REGISTRY}/nginx:${env.BUILD_NUMBER}"
                    }
                }
            }
        }
        stage('Deploy Deployments') {
            steps {
                script {
                    // Apply the database PVC and secret
                    sh '''      
                    kubectl apply -f Deployments/ -n ${NAMESPACE}
                    '''
                }
            }
        }
        stage('Deploy Volumes') {
            steps {
                script {
                    // Apply the proxy deployment and service
                    sh '''
                        kubectl apply -f Volumes/ -n ${NAMESPACE}
                    '''
                }
            }
        }
        stage('Deploy Services') {
            steps {
                script {
                    // Apply the backend deployment and service
                    sh '''
                        kubectl apply -f Services/ -n ${NAMESPACE}
                    '''
                }
            }
        }
    }
}