pipeline {
    agent any

    stages {
        /*
        stage('build') {
            agent{
                docker{
                    image "node:18-alpine"
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
                '''
            }

        }
        */

        stage('test'){
            agent{
                docker{
                    image "node:18-alpine"
                    reuseNode true
                }
            }
            steps{
                sh """
                    test ./build/index.html
                    npm test   
                """
            }
        }

           stage('E2E'){
            agent{
                docker{
                    image "mcr.microsoft.com/playwright:v1.39.0-jammy"
                    reuseNode true
                }
            }
            steps{
                sh """
                    npm install serve
                    node_modules/.bin/serve -s build &
                    #it takes a while for the server to be ready
                    sleep 10
                    npx playwright test --reporter=html   
                """
            }
        }
    }
    post{
        always{
            junit "jest-results/junit.xml"
        }
    } 
}
