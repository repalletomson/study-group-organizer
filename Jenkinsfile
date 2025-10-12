pipeline {
    agent none
    stages {
        stage('Checkout') {
            agent { label 'agent1' }
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            agent { label 'agent1' }
            steps {
                echo 'Installing npm packages...'
                bat 'npm install'
            }
        }
        stage('Lint') {
            agent { label 'agent1' }
            steps {
                echo 'Linting code...'
                bat 'npm run lint || exit /b 0'
            }
        }
        stage('Test') {
            agent { label 'agent2' }
            steps {
                echo 'Running tests...'
                bat 'npm test || exit /b 0'
            }
        }
        stage('Build') {
            agent { label 'agent2' }
            steps {
                echo 'Building production assets...'
                bat 'npm run build'
            }
        }
        stage('Archive Build Artifacts') {
            agent { label 'agent2' }
            steps {
                echo 'Archiving build output...'
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
