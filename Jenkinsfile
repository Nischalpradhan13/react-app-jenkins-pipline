pipeline {
    agent any

    environment {
        DEPLOY_PATH = "/var/www/react-app" // Define the deployment path as an environment variable
    }

    stages {
        stage("Build") {
            steps {
                // Install dependencies and build the React application
                sh "sudo npm install"
                sh "sudo npm run build"
            }
        }
        stage("Deploy") {
            steps {
                // Clean up the existing deployment directory and copy new build files
                sh "echo 'Removing old files from ${DEPLOY_PATH}'"
                sh "sudo rm -rf ${DEPLOY_PATH}"
                
                sh "echo 'Copying new files to ${DEPLOY_PATH}'"
                sh "sudo cp -r '${WORKSPACE}/build' ${DEPLOY_PATH}"
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
