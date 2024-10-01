pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rosh2912/devops-app:${env.BUILD_NUMBER}"
        AWS_REGION = "us-east-1" // Change as per your setup
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/rosh2912/python-project.git'
            }
        }

        stage('Build') {
            steps {
                sh 'python -m venv venv'
                sh 'source venv/Scripts/activate && pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                // Add testing steps if any
                echo 'Running tests...'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    dockerImage = docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Docker Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        dockerImage.push()
                    }
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