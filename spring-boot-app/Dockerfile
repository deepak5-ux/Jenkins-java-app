# Runtime image for Spring Boot app
FROM eclipse-temurin:17-jre-alpine

ARG JAR_FILE=target/spring-boot-web.jar

WORKDIR /opt/app

COPY ${JAR_FILE} app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
