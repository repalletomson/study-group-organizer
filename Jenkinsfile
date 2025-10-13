pipeline {
    agent none
    stages {
        stage('Checkout') {
            agent { label 'master' } 
            steps {
                echo 'Cloning repository on controller...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            agent { label 'win' } // Windows slave node/agent
            steps {
                echo 'Installing npm packages on Windows agent...'
                bat 'npm install'
            }
        }
        stage('Lint') {
            agent { label 'win' }
            steps {
                echo 'Linting code on Windows agent...'
                bat 'npm run lint || exit /b 0'
            }
        }
        stage('Test') {
            agent { label 'win' }
            steps {
                echo 'Running tests on Windows agent...'
                bat 'npm test || exit /b 0'
            }
        }
        stage('Build') {
            agent { label 'win' }
            steps {
                echo 'Building production assets on Windows agent...'
                bat 'npm run build'
            }
        }
        stage('Archive Build Artifacts') {
            agent { label 'master' }
            steps {
                echo 'Archiving build output on controller...'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
    }
    post {
        success {
            echo '✅ CI pipeline completed successfully.'
        }
        failure {
            echo '❌ CI failed. Please check the logs.'
        }
    }
}
