// pipeline {
//     agent any

//     environment {
//         MONGODB_URI = credentials('mongodb-uri')
//         NODE_ENV = 'production'
//         // Add your Docker registry credentials if needed
//         DOCKER_REGISTRY = 'tranvuquanganh87'
//         DOCKER_CREDENTIALS = credentials('docker-credentials')
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Detect Changes') {
//             steps {
//                 script {
//                     // Get changed files between current and previous successful buåååååååild
//                     def changes = []
//                     try {
//                         changes = sh(
//                             script: 'git diff --name-only HEAD^ HEAD',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     } catch (err) {
//                         // If first commit or other git errors, assume all files changed
//                         changes = sh(
//                             script: 'git ls-files',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     }

//                     env.BACKEND_CHANGED = changes.findAll { it.startsWith('server/') }.size() > 0
//                     env.FRONTEND_CHANGED = changes.findAll { it.startsWith('client/') }.size() > 0
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             parallel {
//                 stage('Backend Dependencies') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//                 stage('Frontend Dependencies') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Run Tests') {
//             parallel {
//                 stage('Backend Tests') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh 'npm test'
//                         }
//                     }
//                 }
//                 stage('Frontend Tests') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run cy:run'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Build') {
//             parallel {
//                 stage('Build Backend') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             script {
//                                 // Build Docker image for backend
//                                 sh """
//                                 docker build -t ${DOCKER_REGISTRY}/backend:${BUILD_NUMBER} .
//                                 docker tag ${DOCKER_REGISTRY}/backend:${BUILD_NUMBER} ${DOCKER_REGISTRY}/backend:latest
//                                 """
//                             }
//                         }
//                     }
//                 }
//                 stage('Build Frontend') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             // Build frontend assets
//                             sh 'npm run build'
//                             script {
//                                 // Build Docker image for frontend
//                                 sh """
//                                 docker build -t ${DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} .
//                                 docker tag ${DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} ${DOCKER_REGISTRY}/frontend:latest
//                                 """
//                             }
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Push Docker Images') {
//             steps {
//                 script {
//                     // Login to Docker registry
//                     sh "echo ${DOCKER_CREDENTIALS_PSW} | docker login -u ${DOCKER_CREDENTIALS_USR} --password-stdin ${DOCKER_REGISTRY}"
                    
//                     if (env.BACKEND_CHANGED == 'true') {
//                         sh """
//                         docker push ${DOCKER_REGISTRY}/backend:${BUILD_NUMBER}
//                         docker push ${DOCKER_REGISTRY}/backend:latest
//                         """
//                     }
//                     if (env.FRONTEND_CHANGED == 'true') {
//                         sh """
//                         docker push ${DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}
//                         docker push ${DOCKER_REGISTRY}/frontend:latest
//                         """
//                     }
//                 }
//             }
//         }

//         // stage('Deploy') {
//         //     steps {
//         //         script {
//         //             if (env.BACKEND_CHANGED == 'true') {
//         //                 // Deploy backend
//         //                 sh """
//         //                 kubectl set image deployment/backend backend=${DOCKER_REGISTRY}/backend:${BUILD_NUMBER}
//         //                 """
//         //             }
//         //             if (env.FRONTEND_CHANGED == 'true') {
//         //                 // Deploy frontend
//         //                 sh """
//         //                 kubectl set image deployment/frontend frontend=${DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}
//         //                 """
//         //             }
//         //         }
//         //     }
//         // }
//     }

//     post {
//         success {
//             echo 'Pipeline completed successfully!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//         always {
//             // Clean up Docker images
//             sh """
//             docker rmi ${DOCKER_REGISTRY}/backend:${BUILD_NUMBER} || true
//             docker rmi ${DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} || true
//             """
//         }
//     }
// }



// pipeline {
//     agent any

//     environment {
//         NODE_ENV = 'production'
//         MONGODB_URI = credentials('mongodb-uri')
//         DOCKER_REGISTRY = 'tranvuquanganh87'  // Your Docker Hub username
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Detect Changes') {
//             steps {
//                 script {
//                     def changes = []
//                     try {
//                         changes = sh(
//                             script: 'git diff --name-only HEAD^ HEAD',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     } catch (err) {
//                         changes = sh(
//                             script: 'git ls-files',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     }

//                     env.BACKEND_CHANGED = changes.findAll { it.startsWith('server/') }.size() > 0
//                     env.FRONTEND_CHANGED = changes.findAll { it.startsWith('client/') }.size() > 0
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             parallel {
//                 stage('Backend Dependencies') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//                 stage('Frontend Dependencies') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Run Tests') {
//             parallel {
//                 stage('Backend Tests') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh 'npm test'
//                         }
//                     }
//                 }
//                 stage('Frontend Tests') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run cy:run'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Build') {
//             parallel {
//                 stage('Build Backend') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//                 stage('Build Frontend') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run build'
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Push Docker Images') {
//             steps {
//                 script {
//                     // Fixed Docker login command to use Docker Hub
//                     withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
//                         sh '''#!/bin/bash
//                             echo "$DOCKER_PASS" | docker login docker.io -u "$DOCKER_USER" --password-stdin
//                         '''
//                     }
                    
//                     if (env.BACKEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER}
//                         """
//                     }
//                     if (env.FRONTEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}
//                         """
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Pipeline completed successfully!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//         always {
//             script {
//                 sh """
//                     docker rmi ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} || true
//                     docker rmi ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} || true
//                 """
//             }
//         }
//     }
// }





// pipeline {
//     agent any

//     tools {
//         nodejs 'NodeJS'  // This name must match the name in Jenkins Global Tool Configuration
//     }

//     environment {
//         NODE_ENV = 'production'
//         MONGODB_URI = credentials('mongodb-uri')
//         DOCKER_REGISTRY = 'tranvuquanganh87'
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Detect Changes') {
//             steps {
//                 script {
//                     def changes = []
//                     try {
//                         changes = sh(
//                             script: 'git diff --name-only HEAD^ HEAD',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     } catch (err) {
//                         changes = sh(
//                             script: 'git ls-files',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     }

//                     env.BACKEND_CHANGED = changes.findAll { it.startsWith('server/') }.size() > 0
//                     env.FRONTEND_CHANGED = changes.findAll { it.startsWith('client/') }.size() > 0
                    
//                     // Print changes for debugging
//                     echo "Changes detected in files: ${changes}"
//                     echo "Backend changed: ${env.BACKEND_CHANGED}"
//                     echo "Frontend changed: ${env.FRONTEND_CHANGED}"
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             parallel {
//                 stage('Backend Dependencies') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh '''
//                         # Install all dependencies including mongoose and testing dependencies
//                         npm install
//                         npm install --save mongoose
//                         npm install --save-dev mongodb-memory-server
//                         npm install --save-dev jest
//                         # Add execute permissions
//                         chmod +x node_modules/.bin/*
//                     '''
//                         }
//                     }
//                 }
//                 stage('Frontend Dependencies') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Run Tests') {
//             parallel {
//                 stage('Backend Tests') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                              // Install Jest explicitly before running tests
//                    sh '''
//                         // # Ensure jest is installed explicitly
//                         // npm install jest --save-dev
//                         //  npm install --save mongoose
//                         // npm install --save-dev mongodb-memory-server
//                         # Run jest using npx
//                         // npm run test
//                         npm run test -- --verbose
//                     '''
//                         }
//                     }
//                 }
//                 stage('Frontend Tests') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run cy:run'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Build') {
//             parallel {
//                 stage('Build Backend') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//                 stage('Build Frontend') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run build'
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Push Docker Images') {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
//                         sh '''#!/bin/bash
//                             echo "$DOCKER_PASS" | docker login docker.io -u "$DOCKER_USER" --password-stdin
//                         '''
//                     }
                    
//                     if (env.BACKEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER}
//                         """
//                     }
//                     if (env.FRONTEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}
//                         """
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Pipeline completed successfully!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//         always {
//             script {
//                 sh """
//                     docker rmi ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} || true
//                     docker rmi ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} || true
//                 """
//             }
//         }
//     }
// }


// pipeline {
//     agent any

//     tools {
//         nodejs 'NodeJS'
//     }

//     environment {
//         NODE_ENV = 'production'
//         MONGODB_URI = credentials('mongodb-uri')
//         DOCKER_REGISTRY = 'tranvuquanganh87'
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }

//         stage('Detect Changes') {
//             steps {
//                 script {
//                     def changes = []
//                     try {
//                         changes = sh(
//                             script: 'git diff --name-only HEAD^ HEAD',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     } catch (err) {
//                         changes = sh(
//                             script: 'git ls-files',
//                             returnStdout: true
//                         ).trim().split('\n')
//                     }

//                     env.BACKEND_CHANGED = changes.findAll { it.startsWith('server/') }.size() > 0
//                     env.FRONTEND_CHANGED = changes.findAll { it.startsWith('client/') }.size() > 0
                    
//                     echo "Changes detected in files: ${changes}"
//                     echo "Backend changed: ${env.BACKEND_CHANGED}"
//                     echo "Frontend changed: ${env.FRONTEND_CHANGED}"
//                 }
//             }
//         }

//         stage('Install Dependencies') {
//             parallel {
//                 stage('Backend Dependencies') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh '''
//                                 npm install
//                                 // npm install --save-dev jest
//                                 // chmod +x node_modules/.bin/*
//                             '''
//                         }
//                     }
//                 }
//                 stage('Frontend Dependencies') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm install'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Run Tests') {
//             parallel {
//                 stage('Backend Tests') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             sh '''
//                                 #!/bin/bash
//                                 echo "Node version:"
//                                 node -v
//                                 echo "NPM version:"
//                                 npm -v
//                                 echo "Running tests..."
//                                 NODE_ENV=test npm run test -- --verbose
//                             '''
//                         }
//                     }
//                 }
//                 stage('Frontend Tests') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run cy:run'
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Build') {
//             parallel {
//                 stage('Build Backend') {
//                     when { environment name: 'BACKEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('server') {
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//                 stage('Build Frontend') {
//                     when { environment name: 'FRONTEND_CHANGED', value: 'true' }
//                     steps {
//                         dir('client') {
//                             sh 'npm run build'
//                             script {
//                                 sh "docker build -t ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} ."
//                             }
//                         }
//                     }
//                 }
//             }
//         }

//         stage('Push Docker Images') {
//             steps {
//                 script {
//                     withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
//                         sh '''#!/bin/bash
//                             echo "$DOCKER_PASS" | docker login docker.io -u "$DOCKER_USER" --password-stdin
//                         '''
//                     }
                    
//                     if (env.BACKEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER}
//                         """
//                     }
//                     if (env.FRONTEND_CHANGED == 'true') {
//                         sh """
//                             docker push ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}
//                         """
//                     }
//                 }
//             }
//         }
//     }

//     post {
//         success {
//             echo 'Pipeline completed successfully!'
//         }
//         failure {
//             echo 'Pipeline failed!'
//         }
//         always {
//             script {
//                 sh """
//                     docker rmi ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} || true
//                     docker rmi ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} || true
//                 """
//             }
//         }
//     }
// }


pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    options {
        timeout(time: 15, unit: 'MINUTES')
        skipDefaultCheckout(true)
    }

    environment {
        NODE_ENV = 'production'
        MONGODB_URI = credentials('mongodb-uri')
        DOCKER_REGISTRY = 'tranvuquanganh87'
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                deleteDir()  // Using deleteDir instead of cleanWs
                checkout scm
            }
        }

        stage('Detect Changes') {
            steps {
                script {
                    // Use git to detect changes more efficiently
                    env.BACKEND_CHANGED = sh(
                        script: 'git diff --name-only HEAD^ HEAD | grep ^server/ || true',
                        returnStatus: true
                    ) == 0
                    env.FRONTEND_CHANGED = sh(
                        script: 'git diff --name-only HEAD^ HEAD | grep ^client/ || true',
                        returnStatus: true
                    ) == 0
                    
                    echo "Backend changed: ${env.BACKEND_CHANGED}"
                    echo "Frontend changed: ${env.FRONTEND_CHANGED}"
                }
            }
        }

        stage('Install and Test') {
            parallel {
                stage('Backend') {
                    when { environment name: 'BACKEND_CHANGED', value: 'true' }
                    stages {
                        stage('Install Backend') {
                            steps {
                                dir('server') {
                                    sh 'npm ci'  // Faster than npm install
                                }
                            }
                        }
                        stage('Test Backend') {
                            steps {
                                dir('server') {
                                    sh 'NODE_ENV=test npm test'
                                }
                            }
                        }
                        stage('Build Backend Image') {
                            steps {
                                dir('server') {
                                    sh "docker build -t ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} ."
                                }
                            }
                        }
                    }
                }
                stage('Frontend') {
                    when { environment name: 'FRONTEND_CHANGED', value: 'true' }
                    stages {
                        stage('Install Frontend') {
                            steps {
                                dir('client') {
                                    sh 'npm ci'  // Faster than npm install
                                }
                            }
                        }
                        stage('Test Frontend') {
                            steps {
                                dir('client') {
                                    sh 'npm run cy:run'
                                }
                            }
                        }
                        stage('Build Frontend') {
                            steps {
                                dir('client') {
                                    sh 'npm run build'
                                    sh "docker build -t ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} ."
                                }
                            }
                        }
                    }
                }
            }
        }

        stage('Push Images') {
            steps {
                script {
                    // Login to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                    }
                    
                    // Push images in parallel
                    parallel(
                        backend: {
                            if (env.BACKEND_CHANGED == 'true') {
                                sh "docker push ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER}"
                            }
                        },
                        frontend: {
                            if (env.FRONTEND_CHANGED == 'true') {
                                sh "docker push ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER}"
                            }
                        }
                    )
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            sh """
                docker rmi ${env.DOCKER_REGISTRY}/backend:${BUILD_NUMBER} || true
                docker rmi ${env.DOCKER_REGISTRY}/frontend:${BUILD_NUMBER} || true
            """
        }
    }
}