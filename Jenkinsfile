pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }

    environment {
        SONAR_PROJECT_KEY = "jenkins-sonarqube-demo"
        SONAR_PROJECT_NAME = "Jenkins SonarQube Demo"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh """
                      sonar-scanner \
                      -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                      -Dsonar.projectName='${SONAR_PROJECT_NAME}' \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=$SONAR_HOST_URL \
                      -Dsonar.login=$SONAR_AUTH_TOKEN
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        success {
            echo '✅ SonarQube Analysis PASSED'
        }
        failure {
            echo '❌ SonarQube Analysis FAILED'
        }
    }
}
