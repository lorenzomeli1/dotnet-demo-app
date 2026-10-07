node {
    stage('Checkout') {
        checkout scm
    }
    stage('Deploy') {
        // Stop old containers and deploy the database and web app[cite: 1]
        catchError(buildResult: 'SUCCESS') {
            sh 'docker compose down'
        }
        sh 'docker compose up -d --build'
    }
    stage('Acceptance Test') {
        // Wait 15 seconds for the database and web app to fully boot
        sleep 15
        // Verify the app is responding with a successful HTTP status code
        sh 'curl -f http://172.16.0.10:8081/'
    }
}