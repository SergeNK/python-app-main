pipeline {
    agent any

    parameters {
        string(name: 'REPO_URL', defaultValue: 'http://git-server:3000/max/python-app.git', description: 'the url')
        string(name: 'REPO_DIR', defaultValue: "${WORKSPACE}/python-app", description: 'the dir'
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Branch to build')
        string(name: 'PYTHON_VERSION', defaultValue: 'python3', description: 'The version of python to use')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run tests?')
    }
    stages {
        stage ('Checkout the specified branch') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: "${params.REPO_URL}"
            }
        }

        stage ('Install dependencies') {
            steps {
                script {
                    dir("${params.REPO_DIR}") {
                        sh '''
                            set -e
                            ${params.PYTHON_VERSION} -m venv venv
                            . venv/bin/activate
                            pip install -r requirements.txt
                        '''
                    }
                }
            }
        }

        stage ('Run tests'){
            steps {
                dir("${params.REPO_DIR}") {
                    sh '''
                        set -e
                        . venv/bin/activate
                        pytest --junitxml=reports/test-results.xml
                    '''

                }
            }

        }

        stage ('Run Application'){
            steps {

                dir("${params.REPO_DIR}") {
                    sh '''
                        set -eo pipefail
                        . venv/bin/activate
                        echo "Flask app will run for 60 seconds..."
                        timeout 60s python app.py || echo "App terminated after timeout, ignoring error"
                    '''

                }
            }
        }
    }
}



https://3000-port-krimgjl273aky57l.labs.kodekloud.com/max/python-app