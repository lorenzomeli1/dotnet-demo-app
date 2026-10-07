node {
    stage('Checkout') {
        checkout scm
    }
    stage('Deploy') {
        catchError(buildResult: 'SUCCESS') {
            sh 'docker compose down'
        }
        sh 'docker compose up -d --build'
    }
    stage('Acceptance Test') {
        sleep 15
        sh 'curl -f http://172.16.0.10:8081/'
    }
}