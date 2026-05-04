pipeline {
    agent any
    
    stages {
        
        stage('Setup Environment') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }
        
        stage('Run Tests') {
            steps {
                sh '''
                . venv/bin/activate
                pytest test_app.py -v --tb=short
                '''
            }
        }
        
        stage('Test Coverage') {
            steps {
                sh '''
                . venv/bin/activate
                pytest test_app.py --cov=app --cov-report=term-missing
                '''
            }
        }
        
        stage('Build Artifacts') {
            steps {
                sh '''
                mkdir -p dist
                cp app.py dist/
                cp test_app.py dist/
                cp requirements.txt dist/
                '''
                
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed! Check logs."
        }
    }
}
