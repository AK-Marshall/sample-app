pipeline {
    agent any 
        enviornment {
            NEW_VERSION = '1.3.0'
        }

    stages {

        stage("build") {
            steps {
                echo "Building the application..."
                echo "Building version ${NEW_VERSION})"
            }
        }
        stage("test") {
            when {
                expression { 
                    env.BRANCH_NAME == "dev"
                    }
            }
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
            echo "build ran"
        }
        success {
            echo "build ran successfully"
        }
        failure {
            echo "build failed"
        }
    }


}