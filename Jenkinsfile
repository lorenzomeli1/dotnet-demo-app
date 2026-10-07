node {
    stage('Checkout') {
        checkout scm
    }
    stage('Deploy') {
        // Download docker-compose directly into the Jenkins workspace and make it executable
        sh 'curl -sSL https://github.com/docker/compose/releases/download/v2.26.1/docker-compose-linux-x86_64 -o docker-compose'
        sh 'chmod +x docker-compose'
        
        catchError(buildResult: 'SUCCESS') {
            sh './docker-compose down'
        }
        sh './docker-compose up -d --build'
    }
    stage('Acceptance Test') {
        sleep 15
        sh 'curl -f http://172.16.0.10:8081/'
    }
}