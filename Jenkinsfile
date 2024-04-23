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

        stage("Test") {
            steps {
                script {
                    try {
                        // Update apt and install npm without sudo
                        sh 'sudo -S apt update < /dev/null'
                        sh 'sudo -S apt install -y npm < /dev/null'
                        
                        // Run npm test
                        sh 'npm test'
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
                            // Run build command (assuming build command is 'npm run build')
                            sh 'npm run build'
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
                sh 'docker build -t 4291_wijerathna .' 
            }
        }
        
    }
}
