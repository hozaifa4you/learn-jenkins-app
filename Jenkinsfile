pipeline {
   agent any

   stages {
      stage('Tests') {
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

                        echo "Running E2E tests..."
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

                        echo "Running unit tests..."
                        npm test
                        '''
               }
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
            environment {
            NETLIFY_AUTH_TOKEN = credentials('netlify-access-token')
            NETLIFY_SITE_ID = credentials('netlify-project-id')
            }
            steps {
            sh '''
                echo "Installing Netlify CLI..."
                npm install netlify-cli
                node_modules/.bin/netlify --version

                echo "Deploying to Netlify..."
                node_modules/.bin/netlify status 
                echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                node_modules/.bin/netlify deploy --dir=build --prod
                echo "Deployment successful"
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
