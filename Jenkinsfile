pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git 'https://github.com/hamzaouakrim123/tp4jenkins.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt --break-system-packages'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'python3 -m pytest test_app.py -v'
            }
        }

        stage('SAST Scan - SonarQube') {
            steps {
                sh '''
                docker run --rm \
                  -e SONAR_HOST_URL="http://host.docker.internal:9000" \
                  -e SONAR_LOGIN="sqp_5295ea4a1e7ff9655cbb534d72e718cb9e487e3a" \
                  -v $WORKSPACE:/usr/src \
                  sonarsource/sonar-scanner-cli
                '''
            }
        }

        stage('SCA Scan - OWASP') {
            steps {
                sh '''
                pip3 install safety --break-system-packages
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
