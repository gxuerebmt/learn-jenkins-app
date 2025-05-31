pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '9d84cbc3-d534-42b1-915a-404334eb8038'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
                node_modules/.bin/netlify --version
                echo "Deploying to production. Site ID:$NETLIFY_SITE_ID"
                node_modules/.bin/netlify --status
                '''      
          }
       }
    }        
   post {
        always {
            junit 'junit.xml'
            }
        }
   
}
