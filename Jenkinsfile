pipeline {
    agent any
    stages {
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
        stage('Test'){

            agent{
                docker{
                image 'node:18-alpine'
                reuseNode true
                }
            }
            
            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
                script{
                    if(fileExists('build/index.html')){
                        echo "File exist build/index.html"
                    }
                    else{
                        echo "File does not exist build/index.html"
                    }
                }
            }
        }
    }
}
