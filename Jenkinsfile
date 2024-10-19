pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "shubham765567/node-app"
    }
    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/your-username/node-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        dockerImage.push("${env.BUILD_ID}")
                    }
                }
            }
        }

        stage('Deploy to Localhost') {
            steps {
                script {
                    sh '''
                    docker stop node-app || true
                    docker rm node-app || true
                    docker run -d -p 3000:3000 --name node-app ${DOCKER_IMAGE}:${env.BUILD_ID}
                    '''
                }
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}


