// pipeline {
//     agent any 
//     environment {
//     NEW_VERSION = '1.3.0'
//     }
//     tools {
//         maven 'Maven'
//     }

//     stages {

//         stage("build") {
//             steps {
//                 echo "Building the application..."
//                 echo "Building version ${NEW_VERSION})"
//                 sh "mvn install"
//             }
//         }
//         stage("test") {
//             when {
//                 expression { 
//                     env.BRANCH_NAME == "dev"
//                     }
//             }
//             steps {
//                 echo "Testing the application"
//             }
//         }
        
//         stage("deploy") {
            
//             steps {
//                 echo "Deploying the application"
//             }
//         }

//     }
//     post {
//         always {
//             echo "build ran"
//         }
//         success {
//             echo "build ran successfully"
//         }
//         failure {
//             echo "build failed"
//         }
//     }


// }

pipeline {
    agent any

    environment {
        APP_VERSION = '1.0.0'  // Change this per build if needed
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out the code"
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building Go binary"
                sh 'Go mod tidy'          // Use your Go executable name
                sh 'Go build -o app .'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests"
                sh 'Go test ./...'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker image"
                sh """
                    Docker build -t sample-app:${env.APP_VERSION} .
                """
            }
        }

        stage('Deploy (optional)') {
            when { branch 'dev' } // Only deploy from dev branch
            steps {
                echo "Deploying Docker image (dummy step)"
                // Uncomment and configure if pushing to a registry:
                // sh 'docker tag sample-app:${env.APP_VERSION} myregistry/sample-app:${env.APP_VERSION}'
                // sh 'docker push myregistry/sample-app:${env.APP_VERSION}'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished"
        }
        success {
            echo "SUCCESS: Build + Test + Docker Build succeeded"
        }
        failure {
            echo "FAILURE: Something went wrong"
        }
    }
}
