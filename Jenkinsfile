pipeline {
    agent any

    stages {
        stage("Build") {
            steps {
                sh "npm install"
                sh "NODE_OPTIONS=--openssl-legacy-provider npm run build"
            }
        }
        stage("Serve") {
            steps {
                // Serve the app directly from the build directory
                sh "npm install -g serve"
                sh "serve -s ${WORKSPACE}/build"
            }
        }
    }

    post {
        success {
            echo "App is being served from the build directory."
        }
        failure {
            echo "Pipeline failed. Please check the logs for more details."
        }
    }
}
