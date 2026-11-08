node {
    stage('Checkout') {
        checkout scm
    }

    stage('Deploy to dmz010 (Production)') {
        sh '''
            ssh -i /home/.ssh/jenkins_dmz010 \
              -o BatchMode=yes \
              -o StrictHostKeyChecking=no \
              vagrant@192.0.2.10 '
                # 1. Haal de nieuwste code op van jouw GitHub repository
                if [ -d /home/vagrant/dotnet-demo-app-deploy/.git ]; then
                    git -C /home/vagrant/dotnet-demo-app-deploy pull --ff-only
                else
                    git clone --depth 1 --branch main \
                        https://github.com/lorenzomeli1/dotnet-demo-app.git \
                        /home/vagrant/dotnet-demo-app-deploy
                fi

                # 2. Bouw de Docker image op de dmz010 server
                sudo docker build -t todoapp-prod:latest /home/vagrant/dotnet-demo-app-deploy/TodoApp

                # 3. Verwijder de oude container (indien die bestaat)
                sudo docker rm -f todoapp-prod 2>/dev/null || true

                # 4. Start de nieuwe container, gekoppeld aan poort 8081 en het .env bestand
                sudo docker run -d \
                    --name todoapp-prod \
                    --restart unless-stopped \
                    --env-file /opt/todoapp/.env \
                    -p 127.0.0.1:8081:8080 \
                    todoapp-prod:latest
            '
        '''
    }
}
