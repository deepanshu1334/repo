pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '83c1fbf2-0d6d-4354-ae24-f88dc4528b4e'
        NETLIFY_AUTH_TOKEN = credentials('netfily-token')
        REACT_APP_VERSION = "1.0.$BUILD_ID"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    docker.image('node:18-alpine').inside {
                        sh '''
                           echo 'hello'
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

        stage('Deploy staging') {
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
                    echo "Deploying to staging. Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build
                '''
            }
        }

        stage('Deploy prod') {
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
                    echo "Deploying to production.Site ID:$NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                 '''
            }
        }
    }
}

