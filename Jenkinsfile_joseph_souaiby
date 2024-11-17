pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    // Check if virtual environment exists
                    if (!fileExists("${env.WORKSPACE}/${VIRTUAL_ENV}")) {
                        if (isUnix()) {
                            sh "python3 -m venv ${VIRTUAL_ENV}"
                        } else {
                            bat "python -m venv ${VIRTUAL_ENV}"
                        }
                    }
                    // Activate virtual environment and install requirements
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && pip install -r requirements.txt"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && pip install -r requirements.txt"
                    }
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && flake8 app.py"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && flake8 app.py"
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh "source ${VIRTUAL_ENV}/bin/activate && pytest"
                    } else {
                        bat "${VIRTUAL_ENV}\\Scripts\\activate && pytest"
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    // Add actual deployment logic here
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
