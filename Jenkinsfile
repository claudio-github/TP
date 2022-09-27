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

            stage ('Deploy no Azure App Service') {
                        steps {
                            script {
                                    withCredentials([
                                        string(credentialsId: 'ARM_CLIENT_ID', variable: 'ARM_CLIENT_ID'),
                                        string(credentialsId: 'ARM_CLIENT_SECRET', variable: 'ARM_CLIENT_SECRET'),
                                        string(credentialsId: 'ARM_TENANT_ID', variable: 'ARM_TENANT_ID')
                                    ]) {
                                        sh 'az login --service-principal --username $ARM_CLIENT_ID --password $ARM_CLIENT_SECRET --tenant $ARM_TENANT_ID'
                                        sh 'az webapp config container set --docker-custom-image-name web01_image:$BUILD_ID --name web01 --resource-group talent-pool-rg'
                                    }
                                }
                            }
                        }
                    }

        }
    }