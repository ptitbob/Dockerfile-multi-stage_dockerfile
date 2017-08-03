# Image / container de build
FROM maven:3.5-jdk-8-alpine as builder
ARG version
ENV version ${version:-1.0.0}
WORKDIR /usr/src/mymaven
RUN git clone -b $version https://github.com/ptitbob/Dockerfile-multi-stage_server.git app
RUN mvn -f /usr/src/mymaven/app/pom.xml clean package

# Image final prévu pour l'execution du serveur
FROM openjdk:8u131-jre-alpine as run
ARG version
ENV version ${version:-1.0.0}
ENV JAR_BUILDED  /usr/src/mymaven/app/target/pingserver-$version.jar
WORKDIR /root/
COPY --from=builder /usr/src/mymaven/app/target/pingserver-$version.jar /root/pingserver.jar
CMD java -jar pingserver.jar