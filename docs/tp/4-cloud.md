---
sidebar_position: 4
---

# Cloud

Le Cloud contient plusieurs conteneurs docker.

![Schéma de l'architecture](../assets/edgefogcloud.drawio.png)

- `kafka-broker` : Le broker reçoit, stocke et transmet les messages produits par les producteurs vers les consommateurs. Il gère aussi la réplication et la répartition des partitions (cette fonctionalité n'est pas utilisée dans ce TP).
- `zookeeper` : Utilisé par Kafka pour la gestion de la configuration, l’élection du leader entre les brokers, et la coordination du cluster.
- `schema-registry` : stocke et gère les schémas de données (souvent en Avro, JSON ou Protobuf). Il permet de valider les messages produits/consommés pour assurer la compatibilité des formats.
- `control-center` : Interface web fournie par Confluent pour surveiller et administrer les clusters Kafka, visualiser les topics, messages, et les performances du système.
- `db` : Une base de données TimescaleDB, une extension de PostgreSQL optimisée pour les séries temporelles. Elle est utilisée pour stocker de manière efficace et permannente les données chronologiques ainsi que les utilisateurs.
- `connect` : Service permettant d’intégrer facilement Kafka à d'autres systèmes (ici la base de données). Il exécute des connecteurs pour lire/écrire des données vers Kafka sans code spécifique.
- `website` : Interface web de visualisation des données récoltées.
- `mqtt` : un broker MQTT qui sert pour les tests sans fog.
