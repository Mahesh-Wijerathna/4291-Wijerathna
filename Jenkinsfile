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
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t maheshwijerathna/my-node-app:%BUILD_NUMBER% ./devops'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'my-node-app2', variable: 'my-node-app2')]) {
    
                    bat'docker login -u maheshwijerathna -p ${62672541ol}'
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push adomicarts/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}