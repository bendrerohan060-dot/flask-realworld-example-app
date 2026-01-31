pipeline {
    agent {
        docker {
            image 'python:3.11'
            args '-u root'
        }
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Python project from GitHub'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing Python dependencies'
                sh '''
                    python --version
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running pytest unit tests'
                sh '''
                    mkdir -p test-results
                    pytest --junitxml=test-results/results.xml
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
