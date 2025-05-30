pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '9d84cbc3-d534-42b1-915a-404334eb8038'
    }

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
               agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
               }
            steps {
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        
         post {
        always {
            junit 'junit.xml'
            }
        }
        }
   
    stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npm install netlify-cli
                   netlify --version
                   echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }
    }
   
}
