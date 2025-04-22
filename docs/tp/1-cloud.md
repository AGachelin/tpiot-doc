---
sidebar_position: 1
---

# Cloud
1. Installer Docker (voir section détaillée [ici](../installation/docker)).
2. Se placer dans le dossier cloud.
3. Dans le fichier `docker-compose.yml`, modifier le service `kafka-broker` modifiez la variable d'environnement `KAFKA_ADVERTISED_LISTENERS` pour que l'ip indiquée corresponde à celle du cloud (de votre ordinateur). Attention un ordinateur peut avoir plusieurs adresses ip. Celle du partage de connexion commencera généralement par `192.168`.
4.  `docker compose up -d` (le téléchargement prend 10 - 15 min)
5. Après quelques instants, la page `http://localhost:9021` devrait afficher une page d'accueil.
6. Rajouter un connecteur :
    1. Aller sur `http://localhost:9021` et cliquer sur le cluster (controlcenter.cluster)
    2. Cliquer sur Connect dans la bare latérale
    3. Cliquer sur le premier cluster (connect-default)
    4. Cliquer sur "Add connector", puis sur "Upload connector config file" et choisir le fichier connector_JdbcSinkConnectorConnector_0_config.json qui est dans le dossier connectors.