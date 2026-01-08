pipeline {
    agent any

    stages {

        stage('Checkout Source') {
            steps {
                checkout scm
            }
        }

        stage('Build & Test') {
            steps {
                dir('springboot-cicd-demo'){
                    bat 'mvn clean test'
                }
            }
        }

        stage('Package JAR') {
            steps {
            dir('springboot-cicd-demo'){
                    bat 'mvn clean package -DskipTests'
                }
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