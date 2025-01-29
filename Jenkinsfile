pipeline {
    agent any

    environment {
        // Remplacez par vos credentials DockerHub
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        DOCKER_IMAGE_NAME = "votre-utilisateur-dockerhub/mon-projet-spring"
        DOCKER_IMAGE_TAG = "latest"
    }

    tools {
        // Spécifiez la version de Maven configurée dans Jenkins
        maven 'Maven 3.9.8'
    }

    stages {
        // Étape 1 : Build du projet Spring Boot avec Maven
        stage('Build Spring Boot Project') {
            steps {
                script {
                    echo "Building Spring Boot project with Maven..."
                    sh "mvn clean package"
                }
            }
        }

        // Étape 2 : Construire l'image Docker
        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image..."
                    sh "docker build -t ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG} ."
                }
            }
        }

        // Étape 3 : Pousser l'image Docker sur DockerHub
        stage('Push Docker Image') {
            steps {
                script {
                    echo "Pushing Docker image to DockerHub..."
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USERNAME} --password-stdin"
                        sh "docker push ${DOCKER_IMAGE_NAME}:${DOCKER_IMAGE_TAG}"
                    }
                }
            }
        }
    }

    // Post-actions (optionnel)
    post {
        success {
            echo "Pipeline succeeded! Docker image pushed to DockerHub."
        }
        failure {
            echo "Pipeline failed. Check the logs for more details."
        }
    }
}