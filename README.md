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
