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
                script {
                    def venvDir = "${WORKSPACE}/venv"
                    echo "Creating virtual environment in ${venvDir}"
                    sh "python3 -m venv ${venvDir}" // Ensure this is executed
                }
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
