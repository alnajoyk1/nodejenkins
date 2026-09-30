pipeline {
    agent any

    environment {
        NODE_VERSION = '20'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out NutriFlow source code...'
                checkout scm
            }
        }

        stage('Check Node.js') {
            steps {
                sh '''
                    echo "Node version:"
                    node --version

                    echo "npm version:"
                    npm --version
                '''
            }
        }

        stage('Install Backend Dependencies') {
            steps {
                dir('backend') {
                    sh '''
                        echo "Installing backend dependencies..."
                        npm ci
                    '''
                }
            }
        }

        stage('Backend Check') {
            steps {
                dir('backend') {
                    sh '''
                        echo "Checking backend..."
                        node --check index.js
                    '''
                }
            }
        }

        stage('Install Frontend Dependencies') {
            steps {
                dir('frontend') {
                    sh '''
                        echo "Installing frontend dependencies..."
                        npm ci
                    '''
                }
            }
        }

        stage('Frontend Build') {
            steps {
                dir('frontend') {
                    sh '''
                        echo "Building React frontend..."
                        npm run build
                    '''
                }
            }
        }

        stage('Archive Frontend Build') {
            steps {
                archiveArtifacts artifacts: 'frontend/dist/**',
                                 fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'NutriFlow Jenkins build completed successfully!'
        }

        failure {
            echo 'NutriFlow Jenkins build failed.'
        }

        always {
            echo 'Jenkins pipeline finished.'
        }
    }
}