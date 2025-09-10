pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
        DOCKER_IMAGE = 'python-devsecops'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Requirements') {
            steps {
                sh "echo 'test'"
                //sh 'apt install python3-flask'
            }
        }

        stage('Lint') {
            steps {
                sh 'flake8 ./* || true'
            }
        }

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Secrets Scan (Gitleaks)') {
            steps {
                sh 'gitleaks detect --source . --no-git || true'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t $DOCKER_IMAGE ."
            }
        }

        stage('Trivy Scan') {
            steps {
                sh "trivy image --exit-code 0 --severity CRITICAL,HIGH $DOCKER_IMAGE"
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy stage placeholder.'
            }
        }
    }
}
