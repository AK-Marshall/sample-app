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
        // example version — you can override per-build if needed
        APP_VERSION = '1.0.0'
    }

    stages {
        stage('Checkout') {
            steps {
                // pull repo from SCM (Jenkins will clone the right branch)
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building Go binary"
                sh 'go mod tidy'              // update dependencies
                sh 'go build -o app .'
            }
        }

        stage('Test') {
            steps {
                echo "Running unit tests"
                sh 'go test ./...'
            }
        }

        stage('Docker Build') {
            steps {
                echo "Building Docker image"
                sh """
                   docker build -t sample-app:${env.APP_VERSION} .
                """
            }
        }

        // optional: push or deploy
        stage('Deploy (optional)') {
            when { branch 'dev' }   // only deploy if on dev branch — adjust as needed
            steps {
                echo "Deploying Docker image (dummy step)"
                // e.g. push to registry or deploy — depends on infra
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
