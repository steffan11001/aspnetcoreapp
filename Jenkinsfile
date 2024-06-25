pipeline {
    agent {
        node {
            label 'windows'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    def branchName = env.GIT_BRANCH
                    checkout([$class: 'GitSCM', branches: [[name: branchName]], userRemoteConfigs: [[url: 'https://github.com/steffan11001/aspnetcoreapp.git']]])
                }
            }
        }
        stage('Restore') {
            steps {
                script {
                    def dotnetHome = tool name: 'dotnet', type: 'DotNetCoreSdkInstaller'
                    env.PATH = "${dotnetHome}:${env.PATH}"
                    bat 'dotnet restore'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    def dotnetHome = tool name: 'dotnet', type: 'DotNetCoreSdkInstaller'
                    env.PATH = "${dotnetHome}:${env.PATH}"
                    bat 'dotnet build --no-restore'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    def dotnetHome = tool name: 'dotnet', type: 'DotNetCoreSdkInstaller'
                    env.PATH = "${dotnetHome}:${env.PATH}"
                    bat 'dotnet test --no-restore --verbosity normal'
                }
            }
        }
    }

    post {
        always {
            junit '**/TestResults/*.xml'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
