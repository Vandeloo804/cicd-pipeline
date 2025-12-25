pipeline {
    agent any

    environment {
          DOCKER_IMAGE = 'vandeloo804/cicd-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Application') {
            steps {
                sh './scripts/build.sh'
            }
        }

        stage('Run Tests') {
            steps {
                sh './scripts/test.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("\( {DOCKER_IMAGE}: \){env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'Vandeloo804') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }
    }

    post {
        always {
            sh "docker rmi \( {DOCKER_IMAGE}: \){env.BUILD_NUMBER} || true"
            sh "docker rmi ${DOCKER_IMAGE}:latest || true"
        }
    }
}
