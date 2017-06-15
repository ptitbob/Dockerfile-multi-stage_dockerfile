# Image / container de build
FROM maven:3.5.0-jdk-8 as builder
WORKDIR /usr/src/mymaven
RUN git clone -b 1.0.0 https://github.com/ptitbob/Dockerfile-multi-stage_server.git app
RUN mvn -f /usr/src/mymaven/app/pom.xml clean package

# Image final prévu pour l'execution du serveur
FROM openjdk:8u131-jre-alpine as run
WORKDIR /root/
COPY --from=builder /usr/src/mymaven/app/target/pingserver-1.0.0.jar /root/
CMD java -jar pingserver-1.0.0.jar