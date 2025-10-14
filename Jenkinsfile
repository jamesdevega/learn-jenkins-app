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
            steps{
                sh '''
                    echo "Test stage"
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
