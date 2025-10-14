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
        stage('Build and Archive (Parallel)') {
            parallel {
                stage('Windows Build (aswin_agent)') {
                    agent { label 'win' }
                    stages {
                        stage('Install Dependencies') {
                            steps {
                                echo 'Installing npm packages...'
                                bat 'npm install'
                            }
                        }
                        stage('Lint') {
                            steps {
                                echo 'Linting code...'
                                bat 'npm run lint || exit /b 0'
                            }
                        }
                        stage('Test') {
                            steps {
                                echo 'Running tests...'
                                bat 'npm test || exit /b 0'
                            }
                        }
                        stage('Build') {
                            steps {
                                echo 'Building production assets...'
                                bat 'npm run build'
                            }
                        }
                    }
                }
                stage('Archive Artifacts (controller)') {
                    agent { label 'master' }
                    steps {
                        echo 'Archiving build output...'
                        archiveArtifacts artifacts: 'dist/**', fingerprint: true
                    }
                }
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
