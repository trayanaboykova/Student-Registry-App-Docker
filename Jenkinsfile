pipeline {
    agent any

    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Audit & Test in Parallel') {
            parallel {
                stage('Security Audit') {
                    steps {
                        // Prevent failure due to audit warnings
                        bat 'npm audit || exit 0'
                    }
                }

                stage('Run Tests') {
                    steps {
                        bat 'npm run test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                input message: 'Approve deployment?', ok: 'Deploy'
                bat 'echo Deploying application...'  // Replace with real deploy logic
            }
        }
    }

    post {
        always {
            echo 'Pipeline completed'
        }
        success {
            echo 'Build succeeded'
        }
        failure {
            echo 'Build failed'
        }
    }
}
