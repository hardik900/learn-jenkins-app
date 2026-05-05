pipeline{
    agent any
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
                    sh """
                        npm install netlify-cli@20.1.1
                        node_modules/.bin/netlify --version
                    """
                }
            }
        }
}
