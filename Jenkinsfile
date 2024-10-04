pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = 'my-python-webapp
        IMAGE_TAG = "${env.BUILD_ID}"
        AWS_ACCOUNT_ID = '425031467739'
        ECR_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPO}"
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
                    dockerImage = docker.build("${ECR_REPO}:${IMAGE_TAG}")
                }
            }
        }
        stage('Login to ECR') {
            steps {
                script {
                    sh """
                        aws ecr get-login-password --region ${AWS_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                script {
                    dockerImage.tag("${ECR_URI}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    dockerImage.push()
                }
            }
        }

    post {
        always {
            cleanWs()
        }
    }
}
