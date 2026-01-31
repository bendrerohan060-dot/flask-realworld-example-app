pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out Python project'
                checkout scm
            }
        }

        stage('Install Python & Dependencies') {
            steps {
                echo 'Installing Python and pytest inside Jenkins container'
                sh '''
                    apt-get update
                    apt-get install -y python3 python3-venv python3-pip
                    python3 --version
                    pip3 install --upgrade pip
                    pip3 install pytest
                '''
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running pytest'
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
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
