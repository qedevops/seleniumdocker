pipeline {
    agent any
    stages {
        stage('Build Jar') {
            steps {
                bat "mvn clean package -DskipTests"
            }
        }
        stage('Build Image') {
            steps {
                bat "docker build -t=brudocker/selenium-docker ."
            }
        }
        stage('Push Image') {
            steps {
               withDockerRegistry([ credentialsId: "DockerCred", url: "" ]) {
                    bat "docker push brudocker/selenium-docker:latest"
                    bat "docker push brudocker/selenium-docker:${BUILD_NUMBER}"
                }
            }
        }
    }
}