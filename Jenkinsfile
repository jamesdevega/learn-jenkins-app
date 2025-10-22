pipeline {
    agent any
    stages {
        /*
        stage('Build') {
            agent{
                docker{
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
        stage('Test'){

            agent{
                docker{
                image 'node:18-alpine'
                reuseNode true
                }
            }

            steps{
                sh '''
                    #test -f build/index.html
                    npm test
                    echo "Install Playwright"
                    npm install playwright@1.39.0-noble
                '''
                
            }
        }

        stage('E2E'){
            
            agent{
                docker{
                image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                reuseNode true
                }
            }

            steps{
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build & 
                    sleep 10
                    npx playwright test
                '''
                
            }
        }
    }
    post{
        always{
            junit 'test-results/junit.xml'
        }
    }
}
//npm install -g serve to start and install a simple webserver
// serve -s build to run the page in the server
