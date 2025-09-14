pipeline {
    agent any

    environment {
        registry = "yourdockerhubusername/yourimagename"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }

    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main', url: 'https://github.com/Priyanksh4h/DSO.git'
            }
        }

        stage('Building Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:latest")
                }
            }
        }

        stage('Security Scan') {
            steps {
                script {
                    // Example using Trivy for security scan
                    sh """
                        docker run --rm \
                        -v /var/run/docker.sock:/var/run/docker.sock \
                        aquasec/trivy:latest image ${registry}:latest
                    """
                }
            }
        }

        stage('Deploying Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Clean up') {
            steps {
                script {
                    sh "docker rmi ${registry}:latest || true"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
