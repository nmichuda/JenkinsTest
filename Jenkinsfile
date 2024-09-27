pipeline {
    agent any
    
    environment {
        VENV_DIR = "${WORKSPACE}/venv"  // Always ensure the venv directory is inside the writable workspace
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Repository checked out successfully.'
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                // Create virtual environment in workspace directory
                sh '''
                    python3 -m venv ${VENV_DIR}  // Create virtual environment inside workspace
                    . ${VENV_DIR}/bin/activate   // Activate the virtual environment
                    pip install -r requirements.txt  // Install dependencies
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run pytest and generate JUnit report
                sh '''
                    . ${VENV_DIR}/bin/activate   // Activate virtual environment
                    pytest --junitxml=test-reports/results.xml  // Run pytest and store results
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
