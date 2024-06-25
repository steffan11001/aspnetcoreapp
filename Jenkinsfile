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
                    bat 'dotnet restore'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    bat 'dotnet build --no-restore'
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
