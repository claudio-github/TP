pipeline {
    agent any

    environment {
        registryName = "containerRegistryTalentPool"
        registryUrl = "containerregistrytalentpool.azurecr.io"
        registryCredential = "AzureContainerRegistry"
    }

    stages {
        stage ('Checkout do codigo') {
            steps {
                script {
                sh 'ls'
                }
            }
        }

        stage ('Construcao da imagem docker') {
            steps {
                script {
                    dockerapp = docker.build("web01_image:${env.BUILD_ID}",
                                             '-f Dockerfile .')
                }
            }
        }


        stage ('Docker push') {
            steps {
                script {
                    docker.withRegistry( "http://${registryUrl}", registryCredential ) {
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }


        }
    }