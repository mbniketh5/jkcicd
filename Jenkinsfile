pipeline {
    agent any

    environment {
        IMAGE_NAME = "static-web"
        CONTAINER_NAME = "static-web-container"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/mbniketh5/jkcicd.git'
            }
        }

        stage('Terraform Init') {
            steps {
                dir('terraform') {
                    sh 'terraform init'
                }
            }
        }

        stage('Terraform Validate') {
            steps {
                dir('terraform') {
                    sh 'terraform validate'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Deploy Container') {
            steps {
                sh """
                docker stop ${CONTAINER_NAME} || true
                docker rm ${CONTAINER_NAME} || true

                docker run -d --name static-web-container \
		  -p 8081:80 static-web
                """
            }
        }
    }

    post {
        success {
            echo 'Website deployed successfully'
        }

        failure {
            echo 'Deployment failed'
        }
    }
}
