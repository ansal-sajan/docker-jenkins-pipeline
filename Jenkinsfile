pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE_NAME = "ansalsajan/apache"
    //    USER_NAME = "ansalsajan"
    //    PASSWODRD = credentials ('docker-hub-password')
    }
    
    stages {
      
        stage ("Login to docker hub") {
            steps {
                script {
                    withCredentials ([usernamePassword(credentialsId: 'docker-hub-password', passwordVariable: 'PASSWORD', usernameVariable: 'USER_NAME')]) {
                        sh "echo \$PASSWORD | sudo docker login --username ${USER_NAME} --password-stdin"
                    }
                }
            }
        }
        
        stage ("Building Image") {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    sh "sudo docker build -t ${DOCKER_IMAGE_NAME}:v${buildNumber}.0 ."
                }
            }
        }
        
        stage ("Push to Docker Hub") {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    sh "sudo docker push ${DOCKER_IMAGE_NAME}:v${buildNumber}.0"
                }
            }
        }
        
        stage ("Discarding Old Container") {
            steps {
                script {
                    sh "sudo docker stop web"
                    sh "sudo docker rm web"
                }
            }
        }
        
        stage ("Starting container") {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    sh "sudo docker run --name web -d -p 80:80 ${DOCKER_IMAGE_NAME}:v${buildNumber}.0"
                }
            }
        }
        
    }
}
