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
                git branch: "${params.BRANCH_NAME}", url: "${params.REPO_URL}"
            }
        }

        stage ('Install dependencies') {
            steps {

                        sh '''
                            set -ex
                            cd ./python-app
                            ${params.PYTHON_VERSION} -m venv venv
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
                        set -eo pipefail
                        cd ./python-app
                        . venv/bin/activate
                        echo "Flask app will run for 60 seconds..."
                        timeout 60s python app.py || echo "App terminated after timeout, ignoring error"
                    '''
            }
        }
    }
}
