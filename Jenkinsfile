pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'mon-organisation/mon-application:latest' // Nom de l'image Docker
        DOCKER_REGISTRY = 'https://registry.hub.docker.com'       // URL du registre Docker (Docker Hub par défaut)
        DOCKER_CREDENTIALS = 'docker-hub-credentials'            // ID des credentials Docker
    }
    stages {
        stage('Cloner le dépôt') {
            steps {
                checkout scm
            }
        }
        stage('Construire l\'image Docker') {
            steps {
                script {
                    echo "Construction de l'image Docker : ${DOCKER_IMAGE}"
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }
        stage('Pousser l\'image Docker') {
            steps {
                script {
                    docker.withRegistry(DOCKER_REGISTRY, DOCKER_CREDENTIALS) {
                        echo "Poussée de l'image Docker : ${DOCKER_IMAGE}"
                        sh "docker push ${DOCKER_IMAGE}"
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
