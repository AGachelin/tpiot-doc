---
sidebar_position: 0
slug: /
---

# Introduction
## Présentation
L'objectif de ce TP est de simuler une architecture couramment utilisée pour l'Internet des Objets (IoT), appelée architecture Edge-Fog-Cloud. Cette architecture est conçue pour traiter et analyser des données en temps réel, tout en optimisant les ressources et en réduisant la latence. Dans ce TP, nous allons mettre en œuvre cette architecture pour tracer, sur une interface web, des données collectées par des capteurs.

### Description de l'architecture
![Schéma de l'architecture](./assets/edgefogcloud.drawio.png)
- **Edge (Carte ESP32)** : La carte ESP32 joue le rôle de l'Edge. Elle est responsable de la collecte des données depuis les capteurs connectés. Ces données sont ensuite transmises via le protocole MQTT au niveau Fog.
- **Fog (Raspberry Pi)** : Le Raspberry Pi agit comme une passerelle Fog. Il héberge un proxy MQTT qui reçoit les données de l'ESP32 et les transmet à un serveur Kafka hébergé dans le Cloud. Le Fog permet de réduire la charge sur le Cloud en effectuant un prétraitement ou un filtrage des données si nécessaire.
- **Cloud (Ordinateur personnel)** : Le Cloud est hébergé sur un ordinateur personnel. Il exécute un serveur Kafka pour centraliser les données reçues. Ces données sont ensuite stockées dans une base de données, prêtes à être exploitées par une interface web. Le transfert est assuré par une instance Kafka Connect. Les données reçues par le broker sont comparées à un shéma, pour assurer leur compatibilité. L’ensemble des composants de l’écosystème Kafka (hors base de données) est géré à travers une interface web appelée Control Center.

### Fonctionnement global
1. La carte ESP32 collecte les données des capteurs et les envoie au Raspberry Pi via le protocole MQTT.
2. Le Raspberry Pi transmet ces données au serveur Kafka dans le Cloud.
3. Les données sont enregistrées dans une base de données pour être accessibles.
4. Une interface web récupère les données depuis la base et les affiche sous forme de graphiques en temps réel.

Cette architecture reflète un scénario typique dans les systèmes IoT modernes, où les données sont traitées à différents niveaux pour optimiser les performances et la scalabilité.


## Recommandations
Pour que tout fonctionne correctement, les 3 machines doivent être connectées au même réseau. Si c'est disponible sur votre ordinateur, il est plus simple de les connecter au partage de connexion de votre ordinateur afin de créer un réseau isolé.