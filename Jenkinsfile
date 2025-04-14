pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/SergeNK/python-app-main.git'
        BRANCH_NAME = 'main'
        PYTHON_VERSION = 'python3'
        RUN_TESTS = 'true'
    }
    stages {
        stage ('Checkout the specified branch') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${REPO_URL}"
            }
        }

        stage ('Install dependencies') {
            steps {

                        sh '''
                            set -ex
                            cd ./python-app
                            ${PYTHON_VERSION} -m venv venv
                            . venv/bin/activate
                            pip install -r requirements.txt
                        '''

            }
        }

        stage ('Run tests'){
            steps {
                    sh '''
                        set -e
                        cd ./python-app
                        . venv/bin/activate
                        pytest --junitxml=reports/test-results.xml
                    '''
            }
        }

        stage ('Run Application'){
            steps {

                    sh '''
                        set -ex
                        cd ./python-app
                        . venv/bin/activate
                        echo "Flask app will run for 60 seconds..."
                        timeout 60s python app.py || echo "App terminated after timeout, ignoring error"
                    '''
            }
        }
    }
}
