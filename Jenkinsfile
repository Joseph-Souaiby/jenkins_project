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
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                    // Activate virtual environment and install requirements
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        pip install -r requirements.txt
                    """
                }
            }
        }
        stage('Lint') {
            steps {
                script {
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        flake8 app.py
                    """
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        set PYTHONPATH=%WORKSPACE%
                        pytest
                    """
                }
            }
        }
        stage('Coverage') {
            steps {
                script {
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        set PYTHONPATH=%WORKSPACE%
                        coverage run --source=. -m pytest
                        coverage report
                        coverage xml -o coverage.xml
                    """
                }
            }
            post {
                always {
                    // Publish coverage report (if you have a coverage plugin)
                    publishCoverage adapters: [coberturaAdapter('coverage.xml')], sourceFileResolver: sourceFiles('NEVER_STORE')
                }
            }
        }
        stage('Security Scan') {
            steps {
                script {
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        bandit -r . -f xml -o bandit_report.xml
                    """
                }
            }
            post {
                always {
                    // Publish bandit report (if you have a plugin to visualize it)
                    recordIssues tools: [bandit(pattern: 'bandit_report.xml')]
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    echo "Deploying application..."
                    // Implement your deployment logic here
                    bat """
                        call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                        python deploy.py
                    """
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
