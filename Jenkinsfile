pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                sh "npm install"
                sh "NODE_OPTIONS=--openssl-legacy-provider npm run build"
            }
        }
        stage("Deploy") {
            steps {
                // Deploy to a subdirectory within the workspace
                sh "rm -rf ${WORKSPACE}/deploy"
                sh "mkdir -p ${WORKSPACE}/deploy"
                sh "cp -r '${WORKSPACE}/build' ${WORKSPACE}/deploy"
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
