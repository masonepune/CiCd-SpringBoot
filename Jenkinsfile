pipeline {
    agent any

    environment {
        IMAGE_NAME = 'springboot-cicd-demo'
        IMAGE_TAG  = 'latest'
    }

    stages {
        stage('Cleanup Docker Container') {
            steps {
                bat '''
                docker stop springboot-cicd-demo || exit 0
                docker rm springboot-cicd-demo || exit 0
                '''
            }
        }
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
                  dir('springboot-cicd-demo'){
                      bat 'docker build -t %IMAGE_NAME%:%IMAGE_TAG% .'
                  }
              }
        }

        stage('Run Docker Container') {
            steps {
            dir('springboot-cicd-demo'){
                    bat '''
                    docker stop springboot-cicd-demo || exit 0
                    docker rm springboot-cicd-demo || exit 0

                    docker run -d ^
                      --name springboot-demo ^
                      %IMAGE_NAME%:%IMAGE_TAG%
                    '''
                }
            }
        }
    }

  post {
      success {
          script {
              dir('springboot-cicd-demo') {
                  bat 'docker images | findstr %IMAGE_NAME%'
                  echo '🐳 Docker image build SUCCESS'
              }
          }
      }

      failure {
          echo '❌ Build FAILED'
      }
  }
}