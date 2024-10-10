pipeline {
    agent any
    environment {
        IMAGE_NAME = 'maven:3.8.4-jdk-11'
    }
    stages {
        stage('Checkout') {
            steps {
        sh 'echo passed'
        git branch: 'feature', url: 'https://github.com/sarath1494/devops-automation.git'
            }
        }
        stage('Build with Maven Docker Image') {
            steps {
                script {
                    docker.image(IMAGE_NAME).inside {
                        sh 'ls -lrth && mvn clean install'
                    }
                }
            }
        }
        stage('Build and Push Docker Image') {
			environment {
				DOCKER_IMAGE = "sarath1494/ci-cd:${BUILD_NUMBER}"
					REGISTRY_CREDENTIALS = credentials('docker-cred')
				}
				steps {
					script {
						sh ' docker build -t ${DOCKER_IMAGE} .'
						def dockerImage = docker.image("${DOCKER_IMAGE}")
						docker.withRegistry('https://index.docker.io/v1/', "docker-cred") {
							dockerImage.push()
						sh ' docker rmi ${DOCKER_IMAGE} && docker rmi maven:3.8.4-jdk-11 '
						}
					}
				}
			}

    }

}
