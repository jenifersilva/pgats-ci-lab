pipeline {
    agent any
    
    tools {
        nodejs '24.1.0'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/jenifersilva/pgats-ci-lab.git'
            }
        }
        
        stage('Install Yarn') {
            steps {
                sh 'npm install -g yarn'
            }
        }
        stage('Install project dependencies') {
            steps {
                dir('pgats-ci-lab') {
                    sh 'yarn'
                }
            }
        }
        stage('Install Playwright browsers') {
            steps {
                dir('pgats-ci-lab') {
                    sh 'yarn playwright install --with-deps'
                }
            }
        }
        stage('Run E2E tests') {
            steps {
                dir('pgats-ci-lab') {
                    sh 'yarn run e2e'
                }
            }
        }
    }
    
    post {
        always {
            archiveArtifacts artifacts: 'playwright-report/**', allowEmptyArchive: true
        }
        failure {
            echo '⚠️ Tests have failed'
        }
        success {
            echo '✅ Tests succeeded'
        }
    }
}