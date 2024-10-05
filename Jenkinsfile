pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "rosh2912/devops-app:${env.BUILD_NUMBER}"
        AWS_REGION = "us-east-1" 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/rosh2912/python-project.git'
            }
        }

        stage('Build') {
            steps {
               sh 'python3 -m venv venv'
               sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
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
        post {
        always {
            cleanWs()
        }
    }
}
}
