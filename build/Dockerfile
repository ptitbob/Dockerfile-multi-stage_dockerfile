# Image / container de build
FROM maven:3.5-jdk-8-alpine as builder
ARG version
RUN apk --update add git openssh && \
    rm -rf /var/lib/apt/lists/* && \
    rm /var/cache/apk/*
WORKDIR /usr/src/mymaven
RUN git clone -b $version https://github.com/ptitbob/Dockerfile-multi-stage_server.git app
RUN mvn -f /usr/src/mymaven/app/pom.xml clean package

# Image final prévu pour l'execution du serveur
FROM openjdk:8u131-jre-alpine as run
ARG version
ENV JAR_BUILDED  /usr/src/mymaven/app/target/pingserver-$version.jar
WORKDIR /root/
COPY --from=builder /usr/src/mymaven/app/target/pingserver-$version.jar /root/pingserver.jar
CMD java -jar pingserver.jar