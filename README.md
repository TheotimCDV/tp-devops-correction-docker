# TP DevOps Correction Docker

Repo de Théotim Colin de Verdière, le TP 3 sur ansible à été réalisé en commun avec Daniel Duval


Correction de la partie Docker du module DevOps.

Réponses aux questions :
1-1	Why should we run the container with a flag -e to give the environment variables?
Cela permet de ne pas associer de valeur aux variables d’environnement dans le Dockerfile et donc en clair dans l’application. 

1-2	Why do we need a volume to be attached to our postgres container?
Cela permet de garder  en sauvegarde les tables et enregistrements sql même lorsqu’on stop le container ou qu’on le supprime

1-3	Document your database container essentials: commands and Dockerfile.
FROM postgres:14.1-alpine : me récupère l’image de postgres
docker volume create pgdata : crée un volume qui va me permettre de faire persister les données de ma base même si elle est supprimer
ENV POSTGRES_DB=db \
   POSTGRES_USER=usr \
   POSTGRES_PASSWORD=pwd : permet de données des valeurs au variables d’environnement qui serviront à l’authentification de la bdd, 
Option alternative et plus sécurisé : 
docker run -d `
  --name test `
  -e POSTGRES_USER=usr `
  -e POSTGRES_PASSWORD=pwd `
  -e POSTGRES_DB=db `
  -p 5432:5432 `
  -v pgdata:/var/lib/postgresql/data `
   les mettre directement dans la commande run le -v permet aussi d’associer mon volume à mon container
docker exec -it postgres_container psql -U usr-d bd : me permet de vérifier la connexion bdd directement dans mon container
COPY SQL/*.sql /docker-entrypoint-initdb.d/ : me permet de copier dans le dossier docker-entrypoint-initdb.d mes script sql contenu dans le dossier SQL

1-5 Why do we need a reverse proxy?
Dans notre cas, avoir un reverse proxy nous permet de rediriger le trafic sur le port 80 et donc de cacher les accès au différentes parties de notre application notamment le backend et la base de données au client.

1-6 Why is docker-compose so important?
Utiliser la commande docker-compose avec un fichier yaml rend la création et la gestion de plusieurs conteneurs plus simple pour nous il permet de lancer nos trois conteneurs en précisant par exemple les ports ou les networks avec une seule commande.

1-7 le fichier commenter est le docker-compose.yaml detp-devops-correction-docker

1-9 Document your publication commands and published images in dockerhub.
docker login :  pour se connecter à docker hub avec un nom d'utilisateur et un mdp

Une fois que nos images sont créés on peut les taguer avec la commande docker tag my-database USERNAME/my-database:1.0
Cela nous permet de l'identifier en lui ajoutant notre nom d'utilisateurs dockerHub et la version dans laquelle notre application est et qui sera appelé par la fonction ::latest

ensuite on push l'image sur DockerHub en lui précisant l'image que l'on veut envoyer :
docker push USERNAME/my-database:1.0  

1-10 Why do we put our images into an online repo?
Cela nous permet d'y avoir accès depuis n'importe quelle poste ou serveur sans avoir besoin d'avoir l'image en local, si elle est en publique sur dockerHub on pourra l'utiliser d'où l'on veut

2-1 What are testcontainers?
Se sont des librairies Java qui permettent de run des containeurs docker pendant des tests. 

2-2 Document your Github Actions configurations.
fichier commenter main.yml

2-3 For what purpose do we need to push docker images?
Ça nous permet de nous assurer que notre image sur dockerHub soit toujours bien la latest et qu'on ai pas de différence entre la version que l'on developpe et la latest sur DockerHub

2-4 Document your quality gate configuration.
mvn -B verify sonar:sonar \  # Exécute Maven en mode batch (-B) et vérifie le projet (verify), puis lance l'analyse SonarQube
  -Dsonar.projectKey=<your-project-key> \  # Définit l'identifiant unique du projet SonarCloud
  -Dsonar.organization=<your-organization> \  # Spécifie l'organisation associée au projet SonarCloud
  -Dsonar.host.url=https://sonarcloud.io \  # Indique l'URL du serveur SonarCloud où envoyer l'analyse
  -Dsonar.login=${{ secrets.SONAR_TOKEN }}  \  # Utilise un token stocké dans les secrets GitHub Actions pour l'authentification
  --file ./simple-api/pom.xml  # Indique le chemin du fichier pom.xml pour exécuter Maven sur le bon projet


3-1 Document your inventory and base commands
Fichier commenter setup.yml

3-2 Document your playbook
Fichier commenter playbook.yml

