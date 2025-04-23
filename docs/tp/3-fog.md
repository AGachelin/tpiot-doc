---
sidebar_position: 3
---

# Fog
1. Préparer la machine qui va héberger le fog (voir section détaillée [ici](../installation/raspberry)).
2. Se placer dans le dossier fog.
3. Dans le fichier `docker-compose.yml`, modifier l'ip dans `KAFKA_MQTT_BOOTSTRAP_SERVERS` pour quelle corresponde à celle du cloud
4.  `docker compose up -d`