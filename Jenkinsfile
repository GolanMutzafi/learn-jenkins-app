pipeline {
    agent any

    stages {
        /*

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
        */

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    # Clean npm cache and remove node_modules
                    npm cache clean --force
                    rm -rf node_modules package-lock.json

                    # Install dependencies
                    npm install --retry=3 --fetch-retry-mintimeout=20000 --fetch-retry-maxtimeout=120000

                    # Verify react-scripts is installed
                    npx react-scripts --version || echo "react-scripts not installed"

                    # Run tests
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.49.1-noble'
                    reuseNode true
                }
            }

            steps {
                /*sh '''
                    npm install serve
                    node_modules/serve -s build &
                    sleep 10
                    npx playwright test
                '''*/
                sh '''
                    sleep 10
                    chmod +x node_modules/.bin/serve
                    node_modules/.bin/serve -s build &
                    npx wait-on http://localhost:3000
                    npx playwright test
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}