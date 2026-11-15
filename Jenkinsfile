pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = '9de2d234-993d-4613-8abe-a83aad80ec2c'
        NETLIFY_AUTH_TOKEN = credentials 'netlify-token'
    }

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                    args '-e HTTP_PROXY=http://http.docker.internal:3128 -e HTTPS_PROXY=http://http.docker.internal:3128 -e http_proxy=http://http.docker.internal:3128 -e https_proxy=http://http.docker.internal:3128 -e npm_config_proxy=http://http.docker.internal:3128 -e npm_config_https_proxy=http://http.docker.internal:3128'
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
                    args '-e HTTP_PROXY=http://http.docker.internal:3128 -e HTTPS_PROXY=http://http.docker.internal:3128 -e http_proxy=http://http.docker.internal:3128 -e https_proxy=http://http.docker.internal:3128 -e npm_config_proxy=http://http.docker.internal:3128 -e npm_config_https_proxy=http://http.docker.internal:3128'
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
                    args '-e HTTP_PROXY=http://http.docker.internal:3128 -e HTTPS_PROXY=http://http.docker.internal:3128 -e http_proxy=http://http.docker.internal:3128 -e https_proxy=http://http.docker.internal:3128 -e npm_config_proxy=http://http.docker.internal:3128 -e npm_config_https_proxy=http://http.docker.internal:3128'
                    reuseNode true
                }
            }
            steps {
                sh '''
                   npx netlify --version
                   node_modules/.bin/netlify status
                '''
            }
        }
    }

    
}