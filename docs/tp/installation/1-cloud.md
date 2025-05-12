---
sidebar_position: 1
---

# Cloud
1. Cloner le dépot git dans un dossier vide. `git clone https://github.com/AChardonnet/TPIOT.git <chemin>`
2. Installer Docker (voir section détaillée [ici](../../installation/docker)).
3. Installer Git (voir section détaillée [ici](../../installation/git)).
4. `git clone https://github.com/AChardonnet/TPIOT.git`
5. Se placer dans le dossier `cloud`.
6. Dans le fichier `docker-compose.yml`, modifier le service `kafka-broker` modifiez la variable d'environnement `KAFKA_ADVERTISED_LISTENERS` pour que l'ip indiquée corresponde à celle du cloud (de votre ordinateur). Attention un ordinateur peut avoir plusieurs adresses ip. Il faut choisir celle qui correspond au bon réseau. Celle du partage de connexion commencera généralement par `192.168`.
7.  `docker compose up -d --build` (le téléchargement prend 10 - 15 min)
8. Après quelques instants, la page `http://localhost:9021` devrait afficher une page d'accueil.