# (DOCKER) Automatisation de la livraison avec Sonar et Jenkins :
Ce repositorie est un tutorial explique comment automatise le build, test, analyse du code et le deployment.

Notre objectif c'est d'assuré que notre pipeline fonctionne à chaque modification du code source.
Une seule commite notre application est deployé dans un conteneur Docker

CI:
Checkout to branch
Compile/Build code
Run Tests
Run SONARQUBE
Create docker image
push the image to docker hub
pull and run the image

#### Etape 1 c'est d'éxécuter les service docker-compose:

Docker-compose  c'est la meilleur solution pour éxécuter deux Conteneur et fonctionner entre eux, on commance par configurer notre application avec le document yaml.

```sh
version: '3.2'     # La version de docker-compose à utiliser
services:          # les service on souhaite le démarrer et fonctionner ensemble
  sonarqube:       # SonarQube pour analyser la qualité et la couverture du code avant de l integré et le mérger.
    build: # spécifiant le chemin vers notre fichier dockerFile, ce dockerFile consttuit le conteeur avant de  l'éxécuter
      context: sonarqube/
    ports: # permet de dire a docker qu'on veut exposer un port de notre machine vers notre conteneur, et  ainsi de le rendre accessible depuis l'éxtérieur.
      - 9000:9000
      - 9092:9092
    container_name: sonarqube # réprésent le nom du conteneur à créer
  jenkins:
    build:
      context: jenkins/
    privileged: true
    user: root
    ports:
      - 8080:8080
      - 50000:50000
    container_name: jenkins
    volumes: # permet de stocker l'ensemble du contenu du dossier dans un disque persistant, donc pouvoir garder les données en local sur notre host.
      - /tmp/jenkins:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
    depends_on: # permet de créer une depedence entre deux conteneur, Docker demarre sonarqube avant de démarrer jenkins.
      - sonarqube
```

Le chemin de docker file pour les Conteneurs est spécifié par context attribute dans le ficiher docker-compose.yml.

Regarder le contenus des répértoire sonarqube et jenkins.

Si on a éxécuter les commande suivant dans le répéroire où se trouve docker-compose.yml, les conteuneur sonarqube et jenkins créer et exécuter.

```sh
MAC-190607-05:~ marouanab$ docker-compose -f docker-compose.yml up --build  # -f : spécifier le YAML compose file, up: créer et demarrer le conteneur

MAC-190607-05:~ marouanab$ docker ps # lister les conteneurs
```



## GITHUB Configuration:
## Jenkins Configuration:
## SonarQube Configuration:
## Jenkins file Configuration:



N'hésite pas à me contacter à 'mar.Abakarim@gmail.com' pour toutes questions ou remarques, Merci.
