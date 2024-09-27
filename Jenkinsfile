pipeline {
    agent any
    
    environment {
        VENV_DIR = "${WORKSPACE}/venv"  // Define the virtual environment directory inside the workspace
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Repository checked out successfully.'
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                // Create virtual environment and install dependencies in the writable workspace directory
                sh '''
                    python3 -m venv ${VENV_DIR}  // Create a virtual environment
                    . ${VENV_DIR}/bin/activate   // Activate the virtual environment
                    pip install -r requirements.txt  // Install dependencies
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run pytest with JUnit test result output
                sh '''
                    . ${VENV_DIR}/bin/activate   // Activate virtual environment
                    pytest --junitxml=test-reports/results.xml  // Run pytest and output results
                '''
            }
        }
    }

    post {
        always {
            // Publish test results (JUnit format)
            junit 'test-reports/results.xml'
        }
        
        success {
            echo 'Build Passed: Tests executed successfully!'
        }
        
        failure {
            echo 'Build Failed: One or more tests failed.'
        }
    }
}
