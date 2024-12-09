pipeline {
    agent any

    environment {
        DOCKER_COMPOSE_FILE = 'docker-compose.yml'
        STACK_NAME = 'cars'
    }

    stages {
        stage('Checkout') {
            steps {
                // Клонируем репозиторий
                git branch: 'main', credentialsId: 'your-credentials-id', url: 'git@github.com:sunluaa/car_holding.git'
            }
        }
        
        stage('Build and Push Docker Images') {
            steps {
                sh '''
                docker-compose -f ${DOCKER_COMPOSE_FILE} build
                docker-compose -f ${DOCKER_COMPOSE_FILE} push
                '''
            }
        }
        
        stage('Deploy to Swarm') {
            steps {
                sh '''
                docker stack deploy -c ${DOCKER_COMPOSE_FILE} ${STACK_NAME}
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}
