node {
    stage('Checkout') {
        checkout scm
    }
    stage('Test') {
        // Voert de unit tests uit zoals beschreven in de README
        sh 'dotnet test'
    }
    stage('Deploy') {
        // Sluit eventuele oude containers en start de nieuwe met Docker Compose
        catchError(buildResult: 'SUCCESS') {
            sh 'docker compose down'
        }
        sh 'docker compose up -d --build'
    }
}