pipeline {
    agent any
    environment {
        DOCKER_USER  = 'bharathiio'
        IMAGE_NAME   = 'nodejsweb-io'
        DOCKER_CREDS = 'docker-credential'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Docker Build and Push') {
            steps {
                script {
                    appImage = docker.build("${DOCKER_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER}")

                    docker.withRegistry('', DOCKER_CREDS) {
                        appImage.push()
                        appImage.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            sh "docker rmi ${DOCKER_USER}/${IMAGE_NAME}:${env.BUILD_NUMBER} || true"
            sh "docker rmi ${DOCKER_USER}/${IMAGE_NAME}:latest || true"
            cleanWs()
        }
    }
}