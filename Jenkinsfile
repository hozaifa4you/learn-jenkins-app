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
            echo '''
            Building...
            node --version
            npm --version

            npm ci
            npm run build
            '''
         }
      }
   }
}
