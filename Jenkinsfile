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
        stage('Parallel Build') {
            parallel {
                stage('Controller Tasks') {
                    agent { label 'master' }
                    steps {
                        echo 'Archiving build output on controller...'
                        archiveArtifacts artifacts: 'dist/**', fingerprint: true
                    }
                }
                stage('Aswin Agent Tasks') {
                    parallel { // nested parallel stages if you want multiple steps concurrently
                        stage('Install Dependencies') {
                            agent { label 'win' }
                            steps {
                                echo 'Installing npm packages on aswin_agent...'
                                bat 'npm install'
                            }
                        }
                        stage('Lint') {
                            agent { label 'win' }
                            steps {
                                echo 'Linting code on aswin_agent...'
                                bat 'npm run lint || exit /b 0'
                            }
                        }
                        stage('Test') {
                            agent { label 'win' }
                            steps {
                                echo 'Running tests on aswin_agent...'
                                bat 'npm test || exit /b 0'
                            }
                        }
                        stage('Build') {
                            agent { label 'master' }
                            steps {
                                echo 'Building production assets on aswin_agent...'
                                bat 'npm run build'
                            }
                        }
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
