pipeline {
    agent any

    stages {

        stage('Lint') {
            steps {
                sh '''
                    python3 -m venv .venv
                    . .venv/bin/activate
                    pip install -q flake8
                    flake8 app.py
                '''
            }
        }

        stage('Unit Test') {
            steps {
                sh '''
                    . .venv/bin/activate
                    pip install -q pytest
                    pytest test_app.py -v
                '''
            }
        }

        stage('Package') {
            steps {
                sh '''
                    zip -r app-${GIT_COMMIT}.zip app.py test_app.py
                '''
            }
        }

        stage('Publish to S3') {
            steps {
                sh '''
                    aws s3 cp app-${GIT_COMMIT}.zip \
                    s3://shweta-devops-exp4/app-${GIT_COMMIT}.zip
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully."
            echo "Artifact: app-${GIT_COMMIT}.zip"
        }

        failure {
            echo "Pipeline failed. Check the stage that failed."
        }
    }
}