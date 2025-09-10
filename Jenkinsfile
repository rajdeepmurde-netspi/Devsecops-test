pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/rajdeepmurde-netspi/Devsecops-test.git', branch: 'main'
            }
        }

        //stage('Install Dependencies') {
          //  steps {
            //    sh 'pip install -r app/requirements.txt'
              
            //}
        }

        stage('Run Lint') {
            steps {
                sh 'flake8 app || true'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'sonar-scanner'
                }
            }
        }

        stage('Secrets Scan (Gitleaks)') {
            steps {
                sh 'gitleaks detect --source . --no-git --config-path .gitleaks.toml || true'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t python-devsecops .'
            }
        }

        stage('Scan Docker Image (Trivy)') {
            steps {
                sh 'trivy image --exit-code 0 --severity HIGH,CRITICAL python-devsecops'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploy step placeholder'
            }
        }
    }

