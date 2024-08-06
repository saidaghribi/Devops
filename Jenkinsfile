pipeline {
    agent any

    environment {
        DOTNET_CLI_HOME = "${JENKINS_HOME}/dotnet"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/saidaghribi/Devops']]
                )
            }
        }

        stage('Restore') {
            steps {
                script {
                    bat 'dotnet restore AirportManagement.sln'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat 'dotnet build AirportManagement.sln --configuration Release'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    bat 'dotnet test AirportManagement.sln --configuration Release'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    // Publier chaque projet individuellement
                    bat 'dotnet publish AM.ApplicationCore/AM.ApplicationCore.csproj --configuration Release --output ./publish/AM.ApplicationCore'
                    bat 'dotnet publish AM.Infrastructure/AM.Infrastructure.csproj --configuration Release --output ./publish/AM.Infrastructure'
                    bat 'dotnet publish AM.UI.Console/AM.UI.Console.csproj --configuration Release --output ./publish/AM.UI.Console'
                    bat 'dotnet publish AM.UI.Web/AM.UI.Web.csproj --configuration Release --output ./publish/AM.UI.Web'
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'publish/**/*', allowEmptyArchive: true
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    // Assurez-vous que Maven est installé et configuré
                    bat 'mvn clean install'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }

        success {
            echo 'Le pipeline a réussi !'
        }

        failure {
            echo 'Le pipeline a échoué.'
        }
    }
}
