pipeline {
   agent any

   stages {
      stage('Build') {
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

            echo "Building..."

            npm ci
            npm run build
            '''
         }
      }

      stage('Test') {
         agent {
            docker {
               image 'node:22-alpine'
               reuseNode true
            }
         }
         steps {
            sh '''
            test -f build/index.html || (echo "Build failed, index.html not found" && exit 1)

            echo "Running tests..."
            npm test
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
