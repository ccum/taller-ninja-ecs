FROM maven:3.6-jdk-8-alpine as builder
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:resolve
COPY src ./src
RUN cd /app && mvn -e -B package -Dmaven.test.skip=true


FROM openjdk:8-jdk-alpine
LABEL maintainer="w.marchanaranda@gmail.com"
WORKDIR /workspace

ENV DOCKERIZE_VERSION v0.6.1
#Podemos descargar dockerize o copiarlo desde un directorio
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-alpine-linux-amd64-$DOCKERIZE_VERSION.tar.gz

ENV host="mysql_server"
ENV port="3306"
ENV database="demobd"
ENV username="root"
ENV password="toor"

COPY --from=builder /app/target/*.jar app.jar

ENTRYPOINT exec java -Djava.security.egd=file:/dev/./urandom -jar /workspace/app.jar
EXPOSE 8080
