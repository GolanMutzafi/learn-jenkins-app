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
        stage('Test'){      //1. see if there is 'index.html' 2. execute some test that this project has.
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                    }
                }
            steps{
                echo "Test stage"
                sh '''
                    test -f build/index.html
                    npm test
                '''
              echo "index.html found!"
            }
        }//in order to execute i need to run "npm test" 
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
    }
}