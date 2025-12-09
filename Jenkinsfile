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
        REGISTRY    = "docker.io"                        // or your registry host
        REPO        = "yourdockerhubusername/sample-app" // replace with your Docker Hub username/repo
        IMAGE_TAG   = "${env.BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/AK-Marshall/sample-app.git', branch: 'master'
            }
        }

        stage('Build Go Binary') {
            steps {
                sh 'go mod tidy || true'
                sh 'go build -o app main.go'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${REPO}:${IMAGE_TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-cred', 
                    usernameVariable: 'DOCKER_USER', 
                    passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login ${REGISTRY} -u $DOCKER_USER --password-stdin"
                    sh "docker push ${REPO}:${IMAGE_TAG}"
                    sh "docker tag ${REPO}:${IMAGE_TAG} ${REPO}:latest"
                    sh "docker push ${REPO}:latest"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
        success {
            echo "✅ Build + Docker image pushed: ${REPO}:${IMAGE_TAG}"
        }
        failure {
            echo "⚠️ Pipeline failed."
        }
    }
}
