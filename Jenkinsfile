pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out Python project'
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo 'Setting up Python virtual environment'
                sh '''
                    python -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt || pip install pytest
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running pytest'
                sh '''
                    . venv/bin/activate
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
