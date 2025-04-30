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

## Ajout de capteurs
Dans ce TP, chaque donnée (humidité et température) a son propre topic mqtt et kafka. Pour ajouter un capteur, il n'est pas nécessaire de créer les topics MQTT et Kafka car leur création est automatique. Cependant, il est nécessaire de paramétrer les interfaces qui connectent MQTT, Kafka et la base de données (MQTT-Proxy et Connect).
1. Pour le Proxy MQTT, il faut modifier ou ajouter des expression regex qui permettent de faire correspondre les topics Kafka et MQTT. Par exemple pour la température, l'expression est `temperature:.*temperature[^_]*`. Dans ce cas, tous les messages dans des topics correspondant avec l'expression `.*temperature[^_]*` seront envoyés dans le topic `temperature`. **Il est Impératif que le nom du topic soit au format JSON** 
2. Pour le connecteur vers la base de données, afficher le site `http://localhost:9021/clusters`, cliquer sur `controlcenter.cluster`, puis sur `Connect` dans la barre latérale. Cliquer sur le cluster `connect-default`, puis sur le connecteur `JdbcSinkConnectorConnector_0`, enfin dans l'onglet `Settings` modifier les topics à envoyer vers la base de données. Il est impératif que les messages envoyés soit de la forme :
```
{
  "schema": {
    "type": "struct",
    "fields": [
      {
        "type": "float",
        "optional": false,
        "field": "temperature"
      },
      {
        "type": "int64",
        "optional": false,
        "field": "timestamp"
      }
    ]
  },
  "payload": {
    "temperature": 24.5,
    "timestamp": 1745943209505
  }
}
```