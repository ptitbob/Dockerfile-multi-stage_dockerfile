> Exemple de Dockerfile multi stage

Ici nous allons builder les version de notre serveur (hebergé sur ce depo github : [ptitbob/Dockerfile-multi-stage_server](https://github.com/ptitbob/Dockerfile-multi-stage_server/))

Mais nous allons utiliser des varibale pour cela

Pour contruire l'image (dans le repertoire build) - pour la version 1.0.0 :

```bash
 $ docker build --build-arg version=1.0.0 -t shipstone/multistage:1.0.0 -t shipstone/multistage:latest .
```

Vous remarquerez que je créer deux tag, celui de la version et celui de la dernière version (`latest`)

Pour la version 1.1.0, c'est tout aussi simple : 

```bash
 $ docker build --build-arg version=1.1.0 -t shipstone/multistage:1.1.0 -t shipstone/multistage:latest .
 ```

Cette commande va d'abord créer l'image necessaire pour le build et l'executer, puis ensuite tout en gardant accessible le premier container, va construire l'image final pour y copier le resultat du build maven.
Une fois ce processus terminé, docker ne gardera que la dernière image et liberera le container et l'image de build.

Pour executer l'image de la dernière version ainsi créée : 

```bash
docker run -p 8080:8080 --name multistage shipstone/multistage:latest
```

équivalent à 

```bash
docker run -p 8080:8080 --name multistage shipstone/multistage:1.1.0
```

un simple : 

```bash
$ docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
shipstone/multistage              1.1.0               9e0116fbecdc        14 seconds ago      95.9MB
shipstone/multistage              latest              9e0116fbecdc        14 seconds ago      95.9MB
shipstone/multistage              1.0.0               1d9a0280d0e5        2 minutes ago       95.9MB
shipstone/multistage              1.1.0-simple        1d9c90b9601e        19 minutes ago      95.9MB
shipstone/multistage              1.0.0-simple        7c25dc566b4a        41 minutes ago      95.9MB
openjdk                           8u131-jre-alpine    58ce9579eac6        2 weeks ago         81.4MB
maven                             3.5.0-jdk-8         66091267e43d        4 weeks ago         620MB
```

Simple ? :) :)
