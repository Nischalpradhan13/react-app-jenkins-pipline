pipeline {
    agent { label "Alma" } // Run the entire pipeline on the agent labeled "Alma"

    environment {
        // Define the Docker image name and tag
        DOCKER_IMAGE = "react-app"
        DOCKER_TAG = "latest"
        // Define the port to expose the app
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
                // Create a Dockerfile if it doesn't exist
                sh '''
                echo "FROM nginx:alpine
                COPY build /usr/share/nginx/html
                EXPOSE 80
                CMD [\"nginx\", \"-g\", \"daemon off;\"]" > Dockerfile
                '''

                // Build the Docker image
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage("Run Docker Container") {
            steps {
                // Stop and remove any existing container with the same name
                sh "docker stop ${DOCKER_IMAGE} || true"
                sh "docker rm ${DOCKER_IMAGE} || true"

                // Run the Docker container
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
