pipeline {
    agent any
    
    stages {
        stage('Checkout the repository') {
            steps {
                checkout scm
            }
        }
        
        stage('Restore application') {
            steps {
                bat 'dotnet restore'
            }
        }
        
        stage('Build project') {
            steps {
                bat 'dotnet build'
            }
        }
        
        stage('Test running') {
            steps {
                bat 'dotnet test --logger "trx;LogFileName=results.trx" --results-directory TestResults'
                bat 'copy TestResults\\results.trx TestResults\\results.xml'
            }
            post {
                always {
                    archiveArtifacts artifacts: 'TestResults/results.xml', allowEmptyArchive: true
                }
            }
        }
    }
    
    post {
        always {
            // Clean workspace after build
            cleanWs()
        }
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
