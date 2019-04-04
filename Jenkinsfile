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
                }
            }
        }
        stage("Pull Latest Image"){
			steps{
				bat "docker pull brudocker/selenium-docker"
			}
		}
		stage("Start Grid"){
			steps{
				bat "docker-compose up -d hub chrome firefox"
			}
		}
		stage("Run Test"){
			steps{
				bat "docker-compose up search-module book-flight-module"
			}
		} 
    }
    post{
		always{
			archiveArtifacts artifacts: 'output/**'
			bat "docker-compose down"
		}
	}
}