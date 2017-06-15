> Exemple de Dockerfile multi stage

Ici nous allons builder la version 1.0.0 de notre serveur (hebergé sur ce depo github : [ptitbob/Dockerfile-multi-stage_server](https://github.com/ptitbob/Dockerfile-multi-stage_server/))

Ce second step est aussi simple que le premier, nous clonons dans le container de build le projet issue du tag 1.1.0.

Pour contruire l'image (dans le repertoire build) :

```bash
docker build -t shipstone/multistage:1.1.0-simple .
```

Cette commande va d'abord créer l'image necessaire pour le build et l'executer, puis ensuite tout en gardant accessible le premier container, va construire l'image final pour y copier le resultat du build maven.
Une fois ce processus terminé, docker ne gardera que la dernière image et liberera le container et l'image de build.

Pour executer l'image ainsi créée : 

```bash
docker run -p 8080:8080 --name multistage-1.1.0 shipstone/multistage:1.1.0-simple
```

un simple : 

```bash
$ docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
shipstone/multistage              1.1.0-simple        1d9c90b9601e        23 seconds ago      95.9MB
shipstone/multistage              1.0.0-simple        7c25dc566b4a        14 minutes ago      95.9MB
openjdk                           8u131-jre-alpine    58ce9579eac6        2 weeks ago         81.4MB
maven                             3.5.0-jdk-8         66091267e43d        4 weeks ago         620MB
```

Permet de visualiser le resultat :) et pour les container : 

```bash
$ docker container ls -a
CONTAINER ID        IMAGE                               COMMAND                  CREATED              STATUS                        PORTS               NAMES
c5422eb46e3c        shipstone/multistage:1.1.0-simple   "/bin/sh -c 'java ..."   About a minute ago   Exited (137) 34 seconds ago                       multistage-1.1.0
6b66b27d5fdd        shipstone/multistage:1.0.0-simple   "/bin/sh -c 'java ..."   11 minutes ago       Exited (137) 11 minutes ago                       multistage-1.0.0
```

Simple ? :) :)
