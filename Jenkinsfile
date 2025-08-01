pipeline {
    agent any

    tools {
        dotnet 'dotnet-sdk'  
    }

    environment {
        BUILD_CONFIG = 'Release'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --configuration $BUILD_CONFIG --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c $BUILD_CONFIG -o ./publish'
            }
        }


        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'publish/**/*', fingerprint: true
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace..."
            cleanWs()
        }
    }
}
