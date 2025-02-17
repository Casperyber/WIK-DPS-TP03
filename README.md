WIK-DPS-TP03 - Ping-Api-Dockerisé avec Docker Compose et Reverse-Proxy
Contexte et objectifs
Ce projet est la troisième étape de la série de TP. Il s'appuie sur le TP02 (où l'on dockerisait une API Node.js/TypeScript renvoyant les en-têtes d'une requête GET sur /ping) et y ajoute :

Docker Compose pour orchestrer l'application entière.
4 répliques du service API.
Un reverse-proxy Nginx qui est le seul service exposé sur l'hôte (port 8080) et qui fait du Load Balancing entre les réplicas.
Modification de l'API pour afficher le nom d'hôte dans les logs/réponses (pour observer l'équilibrage de charge).
Prérequis
Node.js v18 (cf. .nvmrc)
npm
Docker

Docker Compose
Inclus avec Docker Desktop ou disponible en tant que plugin pour Docker Engine.

Tester l'API :
Requête GET sur /ping(doit retourner 200 OK et inclure le hostname) :
curl -X GET -i http://localhost:8080/ping
Requête POST sur /ping(doit retourner 404 Not Found) :
curl -X POST -i http://localhost:8080/ping
Orchestration avec Docker Compose
Le fichier docker-compose.yamlorchestre deux services :

api : Construit à partir du Dockerfile (le même que TP02), déployé en 4 réplicas (ne publiée pas de port directement).
reverse-proxy : Utiliser l'image nginx:alpineavec une configuration personnalisée ( nginx.conf) pour faire du Load Balancing vers l'API de service. Seul ce service est exposé sur l'hôte via le port 8080.
Pour lancer l'environnement complet :
Construire et lancer les services en arrière-plan :

sudo docker compose up --build -d
Vérifier l'état des conteneurs :

sudo docker compose ps
Vous devriez voir 4 conteneurs pour le service apiet 1 conteneur pour le service reverse-proxy.

Tester le Reverse-Proxy : Envoyez une requête GET sur /ping:
Tester l'API :
Requête GET sur /ping(doit retourner 200 OK et inclure le hostname) :
curl -X GET -i http://localhost:8080/ping
Requête POST sur /ping(doit retourner 404 Not Found) :
curl -X POST -i http://localhost:8080/ping
Orchestration avec Docker Compose
Le fichier docker-compose.yamlorchestre deux services :

curl -X GET -i http://localhost:8080/ping
La réponse doit inclure le nom d'hôte du conteneur API qui a traité la requête, démontrant ainsi l'équilibrage de charge.

Arrêter l'environnement :
sudo docker compose down
