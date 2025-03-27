# Cours de virtualisation

## Introduction

### Définitions

- **Image** : un logiciel ou un service (sauvegarde)
- **Conteneur** : une instance d'une image (exécution de cette sauvegarde)
- **DockerHub** : registre qui permet de partager des images

### Commandes docker de base

- `docker pull <image>` : télécharger une image depuis DockerHub
- `docker images` : lister les images téléchargées
- `docker run <image>` : exécuter une image
- `docker run –it ubuntu bash` :
  - `-it` : ouvrir un terminal interactif
  - `ubuntu` : nom de l’image à exécuter
  - `bash` : nom de la commande à exécuter sur l’image (ici bash pour avoir un accès au terminal)
- `docker run -it --detach --name <nom> ubuntu bash` : exécuter une image en arrière-plan
  - `--detach` : exécuter en arrière-plan
  - `--name` : donner un nom au conteneur
- `docker exec -it <conteneur> bash` : accéder à un conteneur en cours d’exécution
- `docker run –d –p 8080:80 nginx` :
  - `-p 8080:80` : lier le port 8080 de la machine hôte vers le port 80 du conteneur
- `docker ps` : lister les conteneurs en cours d’exécution
- `docker ps -a` : lister tous les conteneurs
- `docker stop <conteneur>` : arrêter un conteneur
- `docker rm <conteneur>` : supprimer un conteneur
- `docker rmi <image>` : supprimer une image

## Dockerfile

### Définition

Un Dockerfile est un fichier texte qui contient une série d’instructions pour créer une image Docker.

### Exemple

```dockerfile
FROM ubuntu:latest

RUN apt-get update

ENV TZ=Europe/Paris

CMD ["bash"]
```

### Commandes

- `FROM` : spécifie l’image de base
- `RUN` : exécute une commande dans le contexte du futur conteneur
- `CMD` : définit la commande par défaut à exécuter lors du démarrage du conteneur (peut être remplacé lors de l’exécution)
  - `CMD ["bash"]`
  - `CMD ["nginx", "-g", "daemon off;"]`
- `ENTRYPOINT` : définit la commande à exécuter lors du démarrage du conteneur (force l’exécution de cette commande, ne peut pas être remplacée)
  - `ENTRYPOINT ["nginx", "-g", "daemon off;"]`
- `COPY` : copie des fichiers depuis le système de fichiers hôte vers le conteneur
  - `COPY <fichier> <destination>`
- `ENV` : définit des variables d’environnement
  - `ENV TZ=Europe/Paris`
- `LABEL` : ajoute des métadonnées à une image, ils seront visibles avec la commande `docker inspect`
  - `LABEL version="1.0"`

### Build

- `docker build -t <nom> .` : construire une image à partir d’un Dockerfile
  - `-t` : donner un nom à l’image
  - `.` : chemin du Dockerfile
- `docker run <nom>` : exécuter l’image créée

## Volumes

### Named volumes

Permet de stocker des données en dehors du conteneur depuis le système hôte.

Pour **créer** un volume en amont : `docker volume create <nom>`  
Pour **monter** un volume : `docker run -v <nom>:/chemin/du/volume`  
Pour **supprimer** un volume : `docker volume rm <nom>`  
Pour **lister** les volumes : `docker volume ls`

Avantages :
- Persistance des données
- Docker gère la compatibilité entre OS
- Partageable entre plusieurs conteneurs

### Bind mounts

Permet de monter un répertoire du système hôte dans le conteneur, ce sont des volumse à la volée.

Pour **monter** un volume : `docker run -v /chemin/du/volume:/chemin/du/volume`

## Multi-stage

Permet de réduire la taille de l’image finale en utilisant plusieurs images intermédiaires.

```dockerfile
# Exemple d'un Dockerfile multi-stage
# Stage 1
FROM hugo:latest as hugo
COPY ./quickstart /srv
WORKDIR /srv
RUN hugo

# Stage 2
FROM nginx:latest as nginx
# On récupère les fichiers générés par Hugo dans le premier stage
COPY --from=hugo /srv/public /usr/share/nginx/html
```

Pour **construire** une image multi-stage : `docker build -t <nom> .`

## Docker-compose

Permet de gérer plusieurs conteneurs en même temps.

### Exemple de fichier `docker-compose.yml`

```yaml
wordpress:
  image: wordpress
  restart: always
  ports:
    - 8080:80
  environment:
    WORDPRESS_DB_HOST: db
    WORDPRESS_DB_USER: exampleuser
    WORDPRESS_DB_PASSWORD: examplepass
    WORDPRESS_DB_NAME: exampledb
  volumes:
    - wordpress:/var/www/html
  depends_on:
    - db

db:
  image: mysql:5.7
  restart: always
  environment:
    MYSQL_DATABASE: exampledb
    MYSQL_USER: exampleuser
    MYSQL_PASSWORD: examplepass
  volumes:
    - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```

### Mot-clés

- `image` : nom de l’image à exécuter
- `restart` : redémarrer le conteneur en cas d’erreur
- `ports` : lier un port de la machine hôte à un port du conteneur
- `build` : chemin du Dockerfile
- `container_name` : nom du conteneur
- `environment` : variables d’environnement
- `depends_on` : dépendances entre les conteneurs
- `volumes` : volumes à monter
- `networks` : réseaux à utiliser

### Commandes

- `docker-compose up` : démarrer les conteneurs
- `docker-compose down` : arrêter les conteneurs
- `docker-compose ps` : lister les conteneurs

