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
        stage('Test') {
            steps {
                script {
                    bat 'dotnet test --no-restore --verbosity normal'
                }
            }
        }
    }

    post {
        success {
            script {
                currentBuild.result = 'SUCCESS'
                echo "Build succeeded! Sending status to GitHub."
                // Utilizarea plugin-ului GitHub pentru a trimite statusul către GitHub
                githubNotify context: 'Jenkins Build', description: 'Build successful', status: 'SUCCESS'
            }
        }
        failure {
            script {
                currentBuild.result = 'FAILURE'
                echo "Build failed! Sending status to GitHub."
                // Utilizarea plugin-ului GitHub pentru a trimite statusul către GitHub
                githubNotify context: 'Jenkins Build', description: 'Build failed', status: 'FAILURE'
            }
        }
    }
}
