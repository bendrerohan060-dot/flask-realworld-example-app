pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Python project from GitHub'
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo 'Setting up Python virtual environment'
                sh '''
                    python3 --version
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running pytest unit tests'
                sh '''
                    . venv/bin/activate
                    mkdir -p test-results
                    python -m pytest --junitxml=test-results/results.xml
                '''
            }
            post {
                always {
                    junit 'test-results/results.xml'
                }
            }
        }
    }

    post {
        success {
            echo 'Python CI Pipeline completed successfully'
        }
        failure {
            echo 'Python CI Pipeline failed'
        }
    }
}
