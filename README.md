# Sargousse_Damien_SISRD_2022_Docker
# Project Docker:
  # Le butte du Project:
    L’objectifs de ce projet est d’intégrer le déploiement d’une API Rest avec Docker afin de pouvoir
    l’intégrer dans un système d’intégration continue.
__________________________________________________________________________

# DockerFiles

   Dans les document ./frontend et ./backend deux dockerfiles identiques :

    # Téléchargement de l'image
    FROM node:13.12.0-alpine
    RUN apk update && apk upgrade

    # Initialisation répertoire de l'application
    WORKDIR /app

    # Ajout variable d'environnement
    ENV PATH /app/node_modules/.bin:$PATH
    
    # Installation dépendences
    COPY package.json .
    RUN yarn install

    # Copie fichiers du projet
    COPY . .
    CMD [ "yarn", "run", "start" ]
    
__________________________________________________________________________

# Docker-compose
Le docker-compose est sous la racine du project.Il va nous serevir pour la création des image docker pour le frondend et le backend:
     
    version: '3'

    services:

    # Service React défini sur le port 3000 de la pachine
    frontend:
    build: ./frontend
    container_name: react-app
    ports:
      - 3000:3000

    # Service mongo
     mongodb:
    image: mongo:latest
    container_name: mongo
    volumes: 
      - 'mongo-data:/data/db'
    networks:
      - backend

     # Service NodeJS avec une variable d'environnement pour interagir avec MongoDB
     backend:
    build: ./backend
    container_name: nodejs
    environment:
      - MONGO_URI=mongodb://mongo:27017/dbdev
    depends_on:
      - mongodb
    networks:
      - backend
    ports:
      - 8080:8080


    # Volume permettant la persistance des données
    volumes:
    mongo-data:

    networks:
    backend:
    driver: bridge
__________________________________________________________________________
# Architecture projet

l'architecture du project :

![Project Docker](https://user-images.githubusercontent.com/105588266/168471950-0818f5e9-5687-4548-81e0-a51dc91d7bbc.PNG)

__________________________________________________________________________

# Build

Lancer le docker-compose:
        
    docker-compose up
