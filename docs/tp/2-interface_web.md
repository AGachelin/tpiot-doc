---
sidebar_position: 2
---

# Interface web

1. Se placer dans le dossier website
2. `docker compose up -d`

## Troubleshooting
En cas d'erreur relative à la base de donnée, exécuter la commande suivante dans le conteneur cloud-website : `php bin/console doctrine:migration:migrate`.