pipeline {
    agent any

    stages {
        stage ('Checkout do codigo') {
            steps {
                script {
                git url: 'https://github.com/claudio-github/TP.git', branch 'master'
                }
            }
        }
    }
}