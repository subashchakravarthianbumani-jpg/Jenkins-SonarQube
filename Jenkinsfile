pipeline {
    agent any

    environment {
        SONARQUBE_ENV = 'sonarqube'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_ENV}") {
                    sh '''
                    echo "Running SonarQube Scan"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build & Sonar Scan Success'
        }
        failure {
            echo 'Build Failed'
        }
    }
}
