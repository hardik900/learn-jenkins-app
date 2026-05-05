pipeline{
    agent any

    environment{
        NETLIFY_SITE_ID = 'b0a65acd-1e4f-4863-9390-0646bebf61ad'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

        stages{
            stage ('build'){
                agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
                steps{
                    sh """
                        ls -la
                        node --version
                        npm --version
                        npm ci
                        npm run build
                        ls -la
                    """
                }
            }

            stage('check docker') {
                steps {
                    sh 'docker version'
                }
            }

            stage('deploy'){
                agent{
                    docker{
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }

                steps{
                    sh '''
                        echo "deploy stage Initiated"
                        npm install netlify-cli@20.1.1
                        node_modules/.bin/netlify --version
                        echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                        node_modules/.bin/netlify deploy --site=$NETLIFY_SITE_ID --auth=$NETLIFY_AUTH_TOKEN --prod --dir=build
                    '''
                }
            }
        }
}
