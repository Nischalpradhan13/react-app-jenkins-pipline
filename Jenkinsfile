pipeline {
    agent { label "Alma" }

    environment {
        DOCKER_IMAGE = "react-app"
        DOCKER_TAG = "latest"
        APP_PORT = "3000"
    }

    stages {
        stage("Build") {
            steps {
                sh "npm install"
                sh "NODE_OPTIONS=--openssl-legacy-provider npm run build"
            }
        }

        stage("Build Docker Image") {
            steps {
                // Use a heredoc to write the Dockerfile correctly
                sh '''
                    cat > Dockerfile <<EOF
FROM nginx:alpine
COPY build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
EOF
                '''
                // Build the Docker image
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage("Run Docker Container") {
            steps {
                sh "docker stop ${DOCKER_IMAGE} || true"
                sh "docker rm ${DOCKER_IMAGE} || true"
                sh "docker run -d --name ${DOCKER_IMAGE} -p ${APP_PORT}:80 ${DOCKER_IMAGE}:${DOCKER_TAG}"
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
            echo "Access the application at: http://<your-server-ip>:${APP_PORT}"
        }
        failure {
            echo "Pipeline failed. Please check the logs for more details."
        }
    }
}
