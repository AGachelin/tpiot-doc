---
sidebar_position: 2
---

# Interface web

1. `docker exec website php bin/console doctrine:migration:migrate` : Migration de la base de données pour qu'elle corresponde aux entités PHP.
2. `docker exec website php bin/console doctrine:fixtures:load` : Création de faux utilisateurs
3. Ajouter un connecteur :
    1. Aller sur `http://localhost:9021` et cliquer sur le cluster (controlcenter.cluster)
    2. Cliquer sur Connect dans la bare latérale
    3. Cliquer sur le premier cluster (connect-default)
    4. Cliquer sur "Add connector", puis sur "Upload connector config file" et choisir le fichier `connector_JdbcSinkConnectorConnector_0_config.json` qui est dans le dossier `cloud/connectors`.