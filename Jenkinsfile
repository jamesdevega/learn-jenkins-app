pipeline {
    agent any
    stages {
        
        stage('Build') {

            steps{
            }
            /*agent{
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
            }*/
        }
        
        stage('Tests'){
            parallel{
                stage('Unit Test'){

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
                        '''
                        
                    }
                    post{
                        always{
                            junit 'jest-results/junit.xml'
                        }
                    }
                }

                stage('E2E'){
                    agent{
                        docker{
                        image 'mcr.microsoft.com/playwright:v1.39.0'
                        reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            echo "Install Playwright"
                            npm install playwright@1.39.0
                            npm install serve
                            node_modules/.bin/serve -s build & 
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                        
                    }
                    post{
                        always{
                            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
                    }
                }
            }
        }

        
    }
    
}
//npm install -g serve to start and install a simple webserver
// serve -s build to run the page in the server
