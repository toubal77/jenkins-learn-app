pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true // utilise le meme workspace, par defaut is false. les fichiers cree seront supprimer (workspace syncro)
                }
            }
            steps {
                sh '''
                echo 'Build stage running inside a Docker container'
                node -v
                npm -v

                echo 'Installing dependencies...'
                npm ci

                echo 'Building the project...'
                npm run build

                echo 'Build completed.'
                echo 'Contents of the build directory:'
                ls -la 
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo 'Starting tests'

                npm test

                echo 'Tests completed.'
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
