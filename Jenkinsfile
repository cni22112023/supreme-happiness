pipeline {
    agent any
    environment {
        // Nom de l'image Docker à créer
        DOCKER_IMAGE = 'mon-organisation/mon-application:latest'
    }
    stages {
        stage('Cloner le code') {
            steps {
                checkout scm
            }
        }
        stage('Construire le jar') {
            steps {
                sh './mvnw clean package -DskipTests' // Utilisez mvnw pour Maven Wrapper
            }
        }
        stage('Construire l\'image Docker') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        sh """
                        docker build -t ${DOCKER_IMAGE} .
                        docker push ${DOCKER_IMAGE}
                        """
                    }
                }
            }
        }
    }
    post {
        always {
            echo 'Pipeline terminé.'
        }
    }
}
