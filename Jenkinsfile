pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code'
                checkout scm
            }
        }

        stage('Setup Python Virtual Environment') {
            steps {
                echo 'Creating Python virtual environment'
                sh '''
                    python3 --version
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements/dev.txt
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running pytest'
                sh '''
                    . venv/bin/activate
                    pytest
                '''
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
