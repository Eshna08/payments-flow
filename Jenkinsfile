
pipeline {
    agent any

    options {
        disableConcurrentBuilds()
        timestamps()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t payments-flow:${BUILD_NUMBER} .'
            }
        }

        stage('Deploy Application') {
            steps {
                sh '''
                    docker stop payments-flow || true
                    docker rm payments-flow || true

                    docker run -d \
                      --name payments-flow \
                      -p 8081:8080 \
                      --restart unless-stopped \
                      payments-flow:${BUILD_NUMBER}
                '''
            }
        }
    }

    post {
        success {
            echo 'Payments Flow deployed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the build console output.'
        }
    }
}
