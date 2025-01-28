pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'votre-utilisateur/mon-application:latest' // Nom de l'image Docker
        DOCKER_REGISTRY = 'https://registry.hub.docker.com'       // URL du registre Docker
        DOCKER_CREDENTIALS = 'docker-hub-credentials'            // ID des credentials Docker
    }
    tools {
        maven 'Maven' // Utilise le Maven configuré dans Jenkins
    }
    stages {
        stage('Cloner le dépôt') {
            steps {
                checkout scm
            }
        }
        stage('Construire le projet Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
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
