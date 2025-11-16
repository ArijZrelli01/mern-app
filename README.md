MERN APP – Conteneurisation & Déploiement
 
Ce projet est une application MERN composée de :

Un frontend React

Un backend Node.js/Express

Une base de données MongoDB

La conteneurisation est assurée via Docker, l’orchestration via Docker Compose, et la partie Kubernetes est couverte par les déploiements (TP4).

- Table des Matières

Technologies Utilisées

Variables d’Environnement

Configuration Docker

Images Docker

Docker Compose

Exécution du Projet

TP – Étapes Réalisées

TP Docker

TP Kubernetes

- Technologies Utilisées

Frontend : React

Backend : Node.js + Express

Base de données : MongoDB

Conteneurisation : Docker

Orchestration : Docker Compose & Kubernetes

- Variables d’Environnement
Client (React)

REACT_APP_API_URL : URL du backend utilisée dans les appels API.

Serveur (Node.js)

MONGO_URI : URI de connexion à MongoDB.

- Configuration de Docker

Le projet contient deux Dockerfiles :

 Client

Construction du projet React en mode production

Build dans une image node:lts-alpine

 Serveur

Installation des dépendances Node.js

Exposition d’un port API (9000)

- Images Docker Utilisées

Frontend : node:lts-alpine

Backend : node:lts-alpine

Base de données : mongo:latest

Ces images sont référencées dans les Dockerfiles et utilisées lors de la construction.

* Docker Compose

Le fichier docker-compose.yml orchestre :

Le client React

Le serveur Node.js

La base MongoDB

Les services partagent un réseau interne et communiquent entre eux.

- Exécution du Projet

Installer Docker et Docker Compose

Cloner le dépôt

Ouvrir un terminal dans le dossier du projet

Lancer le système complet :

docker-compose up --build


Accéder au client :
 http://localhost:3000

 TP – Étapes Réalisées
 TP Docker
1. Création des Dockerfiles

Dockerfile du serveur

Dockerfile du client

2. Construction des images
   docker build -t mern-server ./server
   docker build -t mern-client ./client

3. Création du réseau docker
   docker network create mern-network

4. Lancement des conteneurs

MongoDB

docker run -d --name mongodb --network mern-network mongo


Serveur

docker run -d --name server --network mern-network -p 9000:9000 mern-server


Client

docker run -d --name client --network mern-network -p 3000:3000 mern-client

5. Création du Docker Compose

Fichier docker-compose.yml regroupant les 3 services

Définition du réseau et des variables d’environnement

6. Lancement complet
   docker-compose up --build


Vérification :

docker-compose ps

** TP Kubernetes (TP4)
- Création des images Docker
-️ Création du ConfigMap
-️ Déploiement MongoDB
-️ Déploiement du serveur 
- ️ Déploiement du client
- Test de communication entre les pods
- Scaling  
- Mise à jour du client (nouvelle version de l’image dans client-deployment)