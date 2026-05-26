pipeline {
    agent any

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
        stage('Test'){
            agent {
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
              sh 'echo "I am in test stage"'
              sh 'test -f build/index.html'
              sh 'npm test'
            }
        }
        stage('E2E'){
            agent {
                docker{
                    image 'mcr.microsoft.com/playwright:v1.39.0-noble'
                    reuseNode true
                }
            }
            steps {
              sh 'echo "I am in E2E stage"'
              sh 'npm install serve'
              sh 'node_modules/.bin/serve -s build &'
              sh 'sleep 10'
              sh 'npx playwright test'
            }
        }
    }
    post {
        always {
            junit 'jest-results/junit.xml'
        }
    }
}