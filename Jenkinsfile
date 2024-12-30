pipeline {
    agent any
    
    environment {
        DOCKER_HUB_CREDS = credentials('docker-hub-credentials')
        DOCKER_IMAGE = 'yourdockerhub/demo-app'
        DOCKER_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh './mvnw test'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push('latest')
                    }
                }
            }
        }
    }
    
    post {
        always {
            junit '**/target/surefire-reports/*.xml'
            cleanWs()
        }
    }
}