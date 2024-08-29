pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            // Needed to create again the docker image
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Test stage'
                // The hash # symbol below works as a comment within a triple single quote multiline string
                sh '''
                    test -f build/index.html
                    ls -la
                    npm test
                '''
            }
        }

        stage('E2E tests') {
            // Needed to create again the docker image
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.46.1-jammy'
                    reuseNode true
                }
            }
            steps {
                echo 'E2E tests stage'
                // The hash # symbol below works as a comment within a triple single quote multiline string
                sh '''
                    npm install serve
                    serve -s build
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
