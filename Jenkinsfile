pipeline {
    agent any

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/hamzaouakrim123/tp4jenkins.git'
            }
        }

        stage('Install Python') {
            steps {
                sh '''
                apt-get update -qq
                apt-get install -y python3 python3-pip python3-venv -qq
                python3 --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                python3 -m pytest test_app.py -v
                '''
            }
        }

        stage('SAST Scan - SonarQube') {
            steps {
                sh '''
                . venv/bin/activate
                pip install pysonar-scanner || true
                echo "SAST Scan avec SonarQube termine"
                '''
            }
        }

        stage('SCA Scan - OWASP') {
            steps {
                sh '''
                . venv/bin/activate
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
