pipeline {
    agent {
        docker {
            image 'python:3.11-slim'
            args '--network host -v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/hamzaouakrim123/tp4jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python -m pytest test_app.py -v'
            }
        }

        stage('SAST Scan - SonarQube') {
            steps {
                sh '''
                pip install pysonar-scanner
                pysonar-scanner \
                  -Dsonar.projectKey=TP4Jenkins \
                  -Dsonar.host.url=http://host.docker.internal:9000 \
                  -Dsonar.login=sqp_5295ea4a1e7ff9655cbb534d72e718cb9e487e3a
                '''
            }
        }

        stage('SCA Scan - OWASP') {
            steps {
                sh '''
                pip install safety
                safety check --full-report || true
                '''
            }
        }

    }

    post {
        success {
            echo 'Pipeline DevSecOps termine avec succes !'
        }
        failure {
            echo 'Build echoue - Verifier les vulnerabilites !'
        }
    }
}
