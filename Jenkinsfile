pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
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
            }
        }

        stage('Test') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh '''
                            test -f build/index.html
                            npm test
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh '''
                            npm install -g netlify-cli
                            netlify --version
                            netlify deploy --dir=build --prod
                        '''
                    }
                }
            }
        }
    }
}
