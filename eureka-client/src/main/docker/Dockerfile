FROM openjdk:8-jre-alpine
VOLUME /tmp
ADD eureka-client-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]