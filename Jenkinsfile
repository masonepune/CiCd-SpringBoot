pipeline {
    agent any

    triggers {
        githubPush()
    }

    stages {

        stage('Checkout Source') {
            steps {
                checkout main
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage('Package JAR') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

          stage('Build Docker Image') {
                    steps {
                        bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
                    }
                }
    }

   post {
        success {
            bat 'docker images | grep $IMAGE_NAME'
            echo '🐳 Docker image build SUCCESS'
        }

        failure {
            echo '❌ Build FAILED'
        }
    }
}