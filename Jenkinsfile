pipeline {
    environment {
    registry = "falug/ast_crm"
    registryCredential = 'DockerHub'
    dockerImage = ''
    }

    agent any
    stages {
            stage('Cloning our Git') {
                steps {

                git branch: 'master',
                    credentialsId: 'github',
                    url: 'https://github.com/Falu-G/springbootrest.git'

                }
            }

            stage('Building Docker Image') {
                steps {
                    script {
                        dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    }
                }
            }

            stage('Deploying Docker Image to Dockerhub') {
                steps {
                    script {
                        docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                        }
                    }
                }
            }

            stage('Cleaning Up') {
                steps{
                  sh "docker rmi --force $registry:$BUILD_NUMBER"
                }
            }
        }
    }
