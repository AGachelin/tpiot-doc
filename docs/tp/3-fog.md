---
sidebar_position: 3
---

# Fog
## Explications
On va mettre en place sur le Raspberry Pi un conteneur docker contenant une instance MQTT Proxy. Il s'agit d'un service permettant de faire la liaison entre MQTT et Kafka.

## Installation
1. Préparer la machine qui va héberger le fog ([voir section détaillée ici](../installation/raspberry)).
2. Copier le dossier fog sur le Raspberry Pi via ssh `scp <Chemin du dossier fog> <user raspberryPi>@<ip raspberryPi>:/home/<user raspberryPi>`
3. Se connecter au Raspberry Pi via ssh `ssh <user raspberryPi>@<ip raspberryPi>`
4. Se placer dans le dossier fog (`cd ./fog`).
5. Dans le fichier `docker-compose.yml`, modifier l'ip dans `KAFKA_MQTT_BOOTSTRAP_SERVERS` pour quelle corresponde à celle du cloud
6. Lancer le stack docker `docker compose up -d`
