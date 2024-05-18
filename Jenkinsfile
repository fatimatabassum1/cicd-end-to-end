pipeline {
    
    agent any 
    
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
    
    stages {
        
        stage('Checkout') {
            steps {
                sh 'echo passed'
                //git branch: 'main', url: 'https://github.com/fatimatabassum1/cicd-end-to-end'
            }
        }

        stage('Build Docker'){
            steps{
                script{
                    sh '''
                    echo 'Build Docker Image'
                    docker build -t fatimatabassum/cicd-new:${BUILD_NUMBER} .
                    '''
                }
            }
        }
        stage('Push Docker Image') {
            environment {
                DOCKER_IMAGE = "fatimatabassum/cicd-new:${BUILD_NUMBER}"
                REGISTRY_CREDENTIALS = credentials('docker-cred')
            }
            steps {
                script {
                    def dockerImage = docker.image("${DOCKER_IMAGE}")
                    docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
                    dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy the container'){
            steps{
                sh 'docker run -d -p 8080:8080 fatimatabassum/jenkins-java-docker'
            }
        }
    }
}
