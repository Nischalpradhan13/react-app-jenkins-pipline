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
                sh "rm -rf /var/www/react-app"
                sh "cp -r ${WORKSPACE}/build/ ."
            }
        }
    }
}
