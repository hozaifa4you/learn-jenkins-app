pipeline {
   agent any

   stages {
      parallel {
         stage('Test E2E') {
         agent {
            docker {
               image 'node:22-alpine'
               reuseNode true
            }
         }
         steps {
            sh '''
            node --version
            npm --version
            echo "Installing dependencies..."
            npm ci

            echo "Running tests..."
            npm test
            '''
         }
         }
      stage('Test Unit') {
         agent {
            docker {
               image 'node:22-alpine'
               reuseNode true
            }
         }
         steps {
            sh '''
            node --version
            npm --version
            echo "Installing dependencies..."
            npm ci

            echo "Running tests..."
            npm test
            '''
         }
      }
      }

      stage('Build') {
         agent {
            docker {
               image 'node:22-alpine'
               reuseNode true
            }
         }
         steps {
            sh '''
            echo "Building..."
            npm run build

            test -f build/index.html || (echo "Build failed, index.html not found" && exit 1)
            '''
         }
      }

      stage('Deploy') {
         agent {
            docker {
               image 'node:22-alpine'
               reuseNode true
            }
         }

         steps {
            sh '''
            echo "Making Ready..."
            npm install netlify-cli -g
            netlify --version

            
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
