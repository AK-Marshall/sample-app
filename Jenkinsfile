pipeline {
    agent any

    environment {
        NEW_VERSION = '1.3.0'
    }

    stages {

        stage("build") {
            steps {
                echo "Building the application version ${NEW_VERSION}"
            }
        }

        stage("test") {
            steps {
                echo "Testing the application"
            }
        }

        stage("deploy") {
            steps {
                echo "Deploying the application"
            }
        }
    }

    post {
        always {
            echo "Build ran"
        }
        success {
            echo "Build ran successfully"
        }
        failure {
            echo "Build failed"
        }
    }
}
