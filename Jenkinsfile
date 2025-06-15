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
         steps {
            sh '''
            test -f build/index.html || (echo "Build failed, index.html not found" && exit 1)
            '''
         }
      }
   }
}
