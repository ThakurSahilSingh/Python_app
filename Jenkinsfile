pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Setup Environment') {
            steps {
                sh '''
                    python3 -m venv $VENV_DIR
                    . $VENV_DIR/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    pytest --maxfail=1 --disable-warnings --tb=short tests/
                '''
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh '''
                    . $VENV_DIR/bin/activate
                    coverage run -m pytest
                    coverage report
                    coverage html
                '''
            }
        }

        stage('Publish Coverage Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'htmlcov',
                    reportFiles: 'index.html',
                    reportName: 'HTML Code Coverage'
                ])
            }
        }
    }
}
