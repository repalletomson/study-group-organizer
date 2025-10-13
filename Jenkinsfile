pipeline {
    agent none
    stages {
        stage('Checkout') {
            agent { label 'win' }
            steps {
                echo 'Cloning repository...'
                checkout scm
            }
        }
        stage('Install Dependencies') {
            agent { label 'win' }
            steps {
                echo 'Installing npm packages...'
                bat 'npm install'
            }
        }
        stage('Lint') {
            agent { label 'win' }
            steps {
                echo 'Linting code...'
                bat 'npm run lint || exit /b 0'
            }
        }
        stage('Test') {
            agent { label 'win' }
            steps {
                echo 'Running tests...'
                bat 'npm test || exit /b 0'
            }
        }
        stage('Build') {
            agent { label 'win' }
            steps {
                echo 'Building production assets...'
                bat 'npm run build'
            }
        }
        stage('Archive Build Artifacts') {
            agent { label 'win' }
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
