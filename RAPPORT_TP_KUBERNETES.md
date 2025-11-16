Rapport TP Kubernetes
Étapes suivies :
Création des ConfigMaps pour la configuration

Déploiement de MongoDB avec service

Déploiement du serveur Express.js

Déploiement du client React

Configuration des services pour l'exposition

Observations détaillées :
Communication entre les pods :
Découverte de service : Les pods du client et serveur communiquent via les noms DNS internes de Kubernetes

Résolution DNS : Le client accède au serveur via server-service (nom du service) plutôt qu'une IP

Isolation réseau : Chaque service crée un point d'accès stable malgré les redémarrages de pods

Load balancing : Le service distribue automatiquement le trafic entre les replicas de pods

ConfigMap et gestion de configuration :
Centralisation : Toutes les variables d'environnement sont gérées dans un seul fichier (app-configmap.yaml)

Découplage : Séparation entre la configuration et le code de l'application

Mise à jour dynamique : Possibilité de modifier la configuration sans reconstruire les images

Montage : Les ConfigMaps sont montés comme volumes ou injectés comme variables d'environnement

Exposition de l'application :
LoadBalancer : Crée un équilibreur de charge externe qui expose l'application sur une IP publique

Port mapping : Translation des ports internes (3000, 5000) vers des ports externes accessibles

Type de services :

ClusterIP pour la communication interne (MongoDB)

LoadBalancer pour l'accès externe (client React)

Réseau : Isolation des services sensibles (MongoDB) de l'exposition publique

Architecture de la base de données :
Isolation : MongoDB déployé dans son propre namespace virtuel via le service

Persistance : Configuration des volumes pour la persistance des données (non montrée mais possible)

Sécurité : Base de données non exposée directement à l'extérieur

Connectivité : Le serveur accède à MongoDB via l'URL interne mongodb-service:27017

Gestion du cycle de vie :
Replicas : Possibilité de scaler horizontalement avec replicas: X dans les deployments

Health checks : Les probes de santé vérifient l'état des applications

Rolling updates : Mises à jour sans interruption de service grâce aux stratégies de déploiement

Auto-healing : Kubernetes redémarre automatiquement les pods défaillants

Sécurité et isolation :
Namespaces : Séparation logique des ressources (optionnelle)

Resource limits : Limitation de la consommation CPU/mémoire pour chaque conteneur

Service accounts : Gestion des permissions d'accès pour les pods

Network policies : Contrôle du trafic réseau entre les pods

