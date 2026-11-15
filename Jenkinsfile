pipeline {
    agent any

    environment {
    NETLIFY_SITE_ID = '9de2d234-993d-4613-8abe-a83aad80ec2c'
    NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    HTTP_PROXY  = 'http://http.docker.internal:3128'
    HTTPS_PROXY = 'http://http.docker.internal:3128'
    NO_PROXY    = 'localhost,127.0.0.1'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18'
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
                    image 'node:18'
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
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli --ignore-scripts

                    node_modules/.bin/netlify --version

                    echo "Deploying to production..."
                    echo "Site ID: 9de2d234-993d-4613-8abe-a83aad80ec2c"

                    node_modules/.bin/netlify deploy \
                    --dir=build \
                    --prod \
                    --site="9de2d234-993d-4613-8abe-a83aad80ec2c"
                '''
    }
}
    }

    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results/junit.xml'
        }
    }
}