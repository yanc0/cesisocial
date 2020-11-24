# Cesi Social

## Testez le lancement de l'application

```bash

# Je télécharge le code
git clone https://github.com/yanc0/cesisocial

# Je me déplace dans le dossier de l'application
cd cesisocial

# je lancer un terminal dans l'image debian
# je monte le dossier courant (pwd) dans le dossier /opt du conteneur
# j'ouvre le port 5000 sur la machine HOST sur le 5000 du conteneur
docker run -v $(pwd):/opt -p 5000:5000 -it debian /bin/bash

# Je suis maintenant dans le conteneur, je vais dans le dossier /opt
cd /opt

# Je vérifie être bien dans le dossier de l'application
ls

# j'installe python et le gestionnaire de dépendance python
apt update
apt install python3 python3-pip --no-install-recommends

# Je déclare les variables d'environnement obligatoires
export FLASK_APP=cesisocial
export FLASK_ENV=development
export PYTHONPATH=/opt

# Je télécharge les dépendances
pip3 install -r requirements.txt

# J'effectue les tests
pytest

# J'initialise un base de données de développement
flask init-db

# Je lance l'application en écoutant sur toutes les IP
flask run -h "0.0.0.0"

```

## Exercice 1


Faire un fichier `Dockerfile` qui permet de créer une image contenant l'application:

* Avec les dépendances
* Les tests ont été exécutés

```Docker
FROM debian:buster

RUN apt update

[...]

EXPOSE 5000
CMD [ "flask", "run", "--host=0.0.0.0" ]
```

Pour créer l'image et la tester, lancer les commandes suivantes:

```
docker build --tag cesisocial:v0.1.0 .
docker run -p 5000:5000 cesisocial:v0.1.0
```

Vérifiez que l'application fonctionne en allant sur: http://127.0.0.1:5000

**Pour quitter l'application, faire Ctrl+C**

## Exercice 2

Créer une branche et ajouter vos modifications

```bash
# Je crée une branche nommée "enable-devops"
git checkout -b enable-devops
# J'ajoute le fichier Dockerfile dans le suivi Git
git add Dockerfile
# Je commite mes modifications
git commit -m "ajout d'un fichier Dockerfile"
```

## Exercice 3

* Créer un compte sur Docker Hub: https://hub.docker.com/signup
* Communiquer votre login Docker au professeur via le chat
* Créer un repository docker publique: `votrelogin/cesisocial`
* Faites une release de votre application en poussant l'image sur le docker hub

```bash
# Je m'authentifie auprès de docker hub
docker login
# Je rebuilde l'application pour tagger l'image avec mon compte docker hub
docker build --tag votrelogin/cesisocial:v0.1.0 .
# Je pousse l'application sur le docker hub
docker push votrelogin/cesisocial:v0.1.0
```

# Exercice 4

* Modifier une partie visible de l'application pour y ajouter votre touche personnelle. Par exemple:
  * Fichier HTML
  * Fichier CSS
* Reconstruire l'application en version `v0.2.0` et la pousser sur le docker hub.
* Demander à un collègue de déployer votre version sur sa machine. L'objectif étant qu'il détermine ce que vous avez modifié sur votre version.

```bash
# Pour tester la version d'un collègue
docker run -p 5000:5000 loginducollegue/cesisocial:v0.2.0

# Pour avoir plusieurs version de la même application sur son poste, changer le port
docker run -p 5001:5000 loginducollegue/cesisocial:v0.2.0
```

# Notes

```bash
# pour faire tourner l'application en tâche de fond
docker run -d -p 5000:5000 loginducollegue/cesisocial:v0.2.0
# pour stopper l'application
docker ps
# copier l'ID et stopper le conteneur
docker stop ID
```