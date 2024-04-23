pipeline {
    agent any 
   
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Mahesh-Wijerathna/4291-Wijerathna'
                }
            }
        }

        // Added Test Stage
        stage('Test') {
            steps {
                script {
                    try {
                        // Update apt and install npm without sudo (adjust based on your needs)
                        sh 'apt update && apt install -y npm'  // Combined commands

                        // Navigate to the directory containing package.json (assuming it's in workspace)
                        dir("${WORKSPACE}") {
                            // Run npm test
                            sh 'npm test'
                        }
                    } catch (err) {
                        echo "Error occurred during testing: ${err}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }
        // Added Build Stage
        stage('Build') {
            steps {
                script {
                    try {
                        // Navigate to the directory containing package.json (assuming it's in workspace)
                        dir("${WORKSPACE}") {
                            // Install dependencies
                            sh 'npm install'
                            // Run build command
                            sh 'npm run build'  // Assuming build command is 'npm run build'
                        }
                    } catch (err) {
                        echo "Error occurred during build: ${err}"
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {  
                sh 'docker build -t 4291-wijerathna .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'DockerHubPassword', variable: 'MaheshDockerHub')]) {
                script{
                    sh 'docker login -u maheshwijerathna -p ${MaheshDockerHub}'
                    }                    
                }
            }
        }
        stage('Push Image') {
            steps {
                sh 'docker push maheshwijerathna/4291-wijerathna'
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}