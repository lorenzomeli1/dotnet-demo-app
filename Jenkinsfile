node {
    stage('Checkout') {
        checkout scm
    }
    stage('Test') {
        sh 'docker run --rm -v jenkins-data:/var/jenkins_home -w /var/jenkins_home/workspace/DotNetPipeline mcr.microsoft.com/dotnet/sdk:10.0 dotnet test'
    }
    stage('Deploy') {
        catchError(buildResult: 'SUCCESS') {
            sh 'docker compose down'
        }
        sh 'docker compose up -d --build'
    }
}