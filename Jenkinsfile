pipeline {
    agent any

    environment {
        DOTNET_CLI_HOME = "${WORKSPACE}/.dotnet"
        DOTNET_ROOT     = "${WORKSPACE}/.dotnet"
        SOLUTION_FILE  = 'Homies.sln'
        BUILD_CONFIG   = 'Release'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                echo 'Restoring NuGet packages...'
                sh "dotnet restore ${SOLUTION_FILE}"
            }
        }

        stage('Build') {
            steps {
                echo "Building solution (${BUILD_CONFIG})..."
                sh "dotnet build ${SOLUTION_FILE} --configuration ${BUILD_CONFIG} --no-restore"
            }
        }

        stage('Run Unit Tests') {
            steps {
                echo 'Running unit tests...'
                sh "dotnet test Homies.Tests/Homies.Tests.csproj --configuration ${BUILD_CONFIG} --no-build --verbosity normal"
            }
        }

        stage('Run Integration Tests') {
            steps {
                echo 'Running integration tests...'
                sh "dotnet test Homies.IntegrationTests/Homies.IntegrationTests.csproj --configuration ${BUILD_CONFIG} --no-build --verbosity normal"
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            cleanWs(deleteDirs: true)
        }
    }
}
