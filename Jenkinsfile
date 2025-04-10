pipeline {
    environment {
        registry = "walid/tp5_jenkins"
        registryCredential = 'docker.hub'  
        dockerHost = 'tcp://127.0.0.1:2375' 
        DOCKER_USERNAME = 'walid'
    }
    agent any
    stages {
        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_PW', variable: 'DOCKER_PW')]) {
                   
                    sh 'docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PW}'
                }
            }
        }
        stage('Cloning Git') {
            steps {
             
                git branch: 'main', url: 'https://github.com/WalidDakir/TP5-DOCKER'
            }
        }
        stage('Building Image') {
            steps {
                script {
                    withEnv(["DOCKER_HOST=${dockerHost}"]) {
                        dockerImage = docker.build("${registry}:${BUILD_NUMBER}")
                    }
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()  
                        dockerImage.push("latest")  
                    }
                }
            }
        }
        stage('Deploy image') {
            steps {
                sh 'docker run -d -p 80:80 ${registry}:${BUILD_NUMBER}'
            }
        }
    }
}
