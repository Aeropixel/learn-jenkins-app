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
    }

    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
