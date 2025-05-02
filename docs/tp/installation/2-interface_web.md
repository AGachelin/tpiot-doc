---
sidebar_position: 2
---

# Interface web

1. `docker exec website php bin/console doctrine:migration:migrate`
2. Rajouter un connecteur :
    1. Aller sur `http://localhost:9021` et cliquer sur le cluster (controlcenter.cluster)
    2. Cliquer sur Connect dans la bare latérale
    3. Cliquer sur le premier cluster (connect-default)
    4. Cliquer sur "Add connector", puis sur "Upload connector config file" et choisir le fichier connector_JdbcSinkConnectorConnector_0_config.json qui est dans le dossier connectors.

## Troubleshooting
En cas d'erreur relative à la base de donnée, exécuter la commande suivante dans le conteneur cloud-website : `php bin/console doctrine:migration:migrate`.