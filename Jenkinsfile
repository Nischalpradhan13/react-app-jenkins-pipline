pipeline {
    agent any

    environment {
        DEPLOY_PATH = "/var/www/react-app" // Define the deployment path as an environment variable
    }

    stages {
        stage("Build") {
            steps {
                // Install dependencies and build the React application
                sh "npm install"
                sh "NODE_OPTIONS=--openssl-legacy-provider npm run build"
            }
        }
        stage("Deploy") {
            steps {
                // Clean up the existing deployment directory
                sh "rm -rf ${DEPLOY_PATH}"

                // Copy new build files to the deployment directory
                sh "cp -r '${WORKSPACE}/build' ${DEPLOY_PATH}"
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully."
        }
        failure {
            echo "Pipeline failed. Please check the logs for more details."
        }
    }
}
