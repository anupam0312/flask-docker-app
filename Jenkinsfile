pipeline {
    agent any

    environment {
        IMAGE_NAME = 'flask-docker-app'
        CONTAINER_NAME = 'flask_app_container'
        DOCKERHUB_USER = 'your-dockerhub-username' // optional if pushing
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'git@github.com:anupam0312/flask-docker-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME} ."
                }
            }
        }

        stage('Stop Previous Container') {
    steps {
        script {
            sh '''
            docker stop flask_app_container || true
            docker rm flask_app_container || true
            docker ps -q --filter "publish=5000" | xargs -r docker stop || true
            '''
        }
    }
}

                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh "docker run -d --name ${CONTAINER_NAME} -p 5000:5000 ${IMAGE_NAME}"
                }
            }
        }

        // Optional: Push to Docker Hub
        /*
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh """
                    echo "$PASSWORD" | docker login -u "$USERNAME" --password-stdin
                    docker tag ${IMAGE_NAME} ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                    docker push ${DOCKERHUB_USER}/${IMAGE_NAME}:latest
                    docker logout
                    """
                }
            }
        }
        */
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Something went wrong.'
        }
    }
}
