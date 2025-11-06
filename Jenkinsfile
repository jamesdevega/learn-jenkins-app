pipeline {
    agent any
    environment{
        /*NETLIFY_SITE_ID is not used anymore, replaced with SITE_ID */
        SITE_ID = '65abecb2-1a01-4fe5-86f0-580365cd8d73'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }
    stages {
        
        stage('Build') {

            steps{
                sh 'echo "Start"'
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

        stage('Deploy') {

            
            agent{
                docker{
                image 'node:18-alpine'
                reuseNode true
                }
            }
            steps {
                //npm install netlify-cli
                sh '''
                
                npm install -g netlify-cli@20.12.2
                node_modules/.bin/netlify --version
                echo "Deploying to production. Site ID: $SITE_ID"
                node_modules/.bin/netlify status
                node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
        
    }
    
}
//npm install -g serve to start and install a simple webserver
// serve -s build to run the page in the server
