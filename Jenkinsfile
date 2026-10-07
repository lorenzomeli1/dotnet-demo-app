node {
    stage('Checkout') {
        checkout scm
    }
    stage('Deploy') {
        sh 'curl -sSL https://github.com/docker/compose/releases/download/v2.26.1/docker-compose-linux-x86_64 -o docker-compose'
        sh 'chmod +x docker-compose'
        
        catchError(buildResult: 'SUCCESS') {
            sh './docker-compose down'
        }
        sh './docker-compose up -d --build'
        
        // Manually initialize the database schema after the containers boot
        sh 'docker exec -i todoappdb mariadb -utodo_usr -pletmeinplz todo_db < TodoApp/schema.sql'
    }
    stage('Acceptance Test') {
        sleep 15
        sh 'curl -f http://172.16.0.10:8081/'
    }
}