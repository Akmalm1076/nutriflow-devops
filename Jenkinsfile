pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'NutriFlow source code checked out from GitHub'
            }
        }

        stage('Backend Validation') {
            steps {
                dir('backend') {
                    sh '''
                        npm ci --omit=dev
                        node --check index.js
                    '''
                }
            }
        }

        stage('Frontend Lint') {
            steps {
                dir('frontend') {
                    sh 'npm ci'
                    script {
                        int lintStatus = sh(
                            script: 'npm run lint',
                            returnStatus: true
                        )

                        if (lintStatus != 0) {
                            currentBuild.result = 'UNSTABLE'
                            echo 'WARNING: Frontend lint failed. Review the ESLint findings.'
                        }
                    }
                }
            }
        }

        stage('Frontend Build') {
            steps {
                dir('frontend') {
                    sh 'npm run build'
                }
            }
        }
    }
}
