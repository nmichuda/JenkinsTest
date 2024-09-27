pipeline {
    agent any
    
    environment {
        VENV_DIR = 'venv'  // Define the virtual environment directory
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the GitHub repository
                git 'https://github.com/nmichuda/JenkinsTest.git'
            }
        }
        
        stage('Set up Python Environment') {
            steps {
                // Install Python virtual environment and dependencies
                sh '''
                    python3 -m venv ${VENV_DIR}  // Create a virtual environment
                    . ${VENV_DIR}/bin/activate    // Activate the virtual environment
                    pip install -r requirements.txt  // Install dependencies from requirements.txt
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                // Run pytest and generate test report
                sh '''
                    . ${VENV_DIR}/bin/activate    // Activate virtual environment
                    pytest --junitxml=test-reports/results.xml  // Run pytest and output JUnit report
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
