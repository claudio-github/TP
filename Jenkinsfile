pipeline {
    agent any

    environment {
        //REGISTRYURL = credentials('REGISTRYURL')
        REGISTRYURL = "containerregistrytp0001.azurecr.io"
        REGISTRYCREDENTIAL = "REGISTRYCREDENTIAL"
        RESOURCE_GROUP = "talentpoolproject0001-rg"
        APPSERVICENAME = "talentpoolproject0001"
        IMAGE_NAME = "web01_image"
        AZURECREDENTIALSID = credentials('AZURECREDENTIALSID')
    }


      options {
        skipDefaultCheckout(true)
    }

    stages {

        stage('Clean workspace') {
            steps{
                script{
                    cleanWs()
                }
            }
        }

          stage('Checkout') {
            steps{
                script{
                    checkout scm
                }
            }
        } 

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
                    dockerapp = docker.build("${IMAGE_NAME}:${env.BUILD_ID}",
                                             '-f Dockerfile .')
                }
            }
        }


        stage ('Docker push') {
            steps {
                script {
                    docker.withRegistry( "http://${REGISTRYURL}", REGISTRYCREDENTIAL ) {
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

            stage ('Deploy no Azure App Service') {
                        steps {
                            script {
                                  /*  withCredentials([
                                        string(credentialsId: 'ARM_CLIENT_ID', variable: 'ARM_CLIENT_ID'),
                                        string(credentialsId: 'ARM_CLIENT_SECRET', variable: 'ARM_CLIENT_SECRET'),
                                        string(credentialsId: 'ARM_TENANT_ID', variable: 'ARM_TENANT_ID')
                                    ]) {*/

                                        azureWebAppPublish azureCredentialsId: '$AZURECREDENTIALSID', publishType: 'docker',
                                                           resourceGroup: '$RESOURCE_GROUP', appName: '$APPSERVICENAME',
                                                           dockerImageName: '$IMAGE_NAME', dockerImageTag: '$BUILD_ID',
                                                           dockerRegistryEndpoint: [credentialsId: 'REGISTRYCREDENTIAL', url: "$REGISTRYURL"]
                                        /*sh 'az login --service-principal --username $ARM_CLIENT_ID --password $ARM_CLIENT_SECRET --tenant $ARM_TENANT_ID'
                                        sh 'az webapp config container set --name $APPSERVICENAME --resource-group $RESOURCE_GROUP --docker-custom-image-name $REGISTRYURL/web01_image:$BUILD_ID'
                                        */
                                   // }
                                }
                            }
                        }
                    }

        }
    