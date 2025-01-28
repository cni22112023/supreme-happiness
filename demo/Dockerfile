# Utiliser une image JDK comme base
FROM openjdk:17-jdk-slim

# Copier le jar dans l'image
COPY target/mon-application.jar app.jar

# Exposer le port par défaut
EXPOSE 8080

# Commande de démarrage
ENTRYPOINT ["java", "-jar", "app.jar"]
