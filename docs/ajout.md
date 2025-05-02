---
sidebar_position: 1
---

# Ajout de capteurs
Dans ce TP, chaque donnée (humidité et température) a son propre topic mqtt et kafka. Pour ajouter un capteur, il n'est pas nécessaire de créer les topics MQTT et Kafka car leur création est automatique. Cependant, il est nécessaire de paramétrer les interfaces qui connectent MQTT, Kafka et la base de données (MQTT-Proxy et Connect). Il faut aussi s'assurer que l'interface web puissse accéder aux données.
## Entité Symfony
Il faut faire en sorte que l'interface web puisse accéder aux données, pour cela il faut créer une entité dans le code symfony. Il existe pour cela une commande (à effectuer dans le dossier `website`) `php bin/console make:entity` (il faut pour l'exécuter avoir installé php). La commande demande le nom de l'entité (ex: `Temperature`) puis le nom des propriétés suivies de leur type (dans notre exemple `temperature` de type `float` puis `timestamp` de type `bigint`). Enfin il faut exécuter la commande `php bin/console make:migration`.
Il faut ensuite modifier le fichier migration créé. Dans la méthode publique `up` il faut supprimer les lignes qui retirent la valeur par défaut des entitées déjà crées (notemment `Temperature` et `Humidity`). Pour cela il faut retirer les instructions :
```
$this->addSql(<<<'SQL'
    ALTER TABLE temperature ALTER id DROP DEFAULT
SQL);
```
Puis il faut ajouter la valeur par défaut de l'id pour celles que l'on vient de créer en modifiant l'instruction de création de la table.
Il faut ajouter `DEFAULT nextval('temperature_id_seq')` à la fin de la définition de la colonne `id`.
Par exemple, la ligne :
```
CREATE TABLE temperature (
    id INT NOT NULL, 
    temperature DOUBLE PRECISION NOT NULL, 
    timestamp BIGINT NOT NULL, 
    PRIMARY KEY(id))
```
Devient:
```
CREATE TABLE temperature (
    id INT NOT NULL DEFAULT nextval('temperature_id_seq'), 
    temperature DOUBLE PRECISION NOT NULL, 
    timestamp BIGINT NOT NULL, 
    PRIMARY KEY(id))
```
## Proxy MQTT
Pour le Proxy MQTT, il faut modifier ou ajouter des expression regex qui permettent de faire correspondre les topics Kafka et MQTT. Par exemple pour la température, l'expression est `temperature:.*temperature[^_]*`. Dans ce cas, tous les messages dans des topics correspondant avec l'expression `.*temperature[^_]*` seront envoyés dans le topic `temperature`. **Il est Impératif que le nom du topic soit au format JSON** 
## Connecteur
Pour le connecteur vers la base de données, accéder au site `http://localhost:9021/clusters`, cliquer sur `controlcenter.cluster`, puis sur `Connect` dans la barre latérale. Cliquer sur le cluster `connect-default`, puis sur le connecteur `JdbcSinkConnectorConnector_0`, enfin dans l'onglet `Settings` modifier les topics à envoyer vers la base de données. Il est impératif que les messages envoyés par l'ESP32 soient de la forme :
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
## Component Twig
Afin d'afficher le graphique des données en temps réel, il faut créer un composant twig (Twig est un moteur de templates pour PHP).
Créer une copie du fichier `website\src\Twig\Components\GenericChart.php` et la nommer, attention le nom de la classe doit être identique au nom du fichier (attention à la casse).
Pour modifier l'entité à afficher modifier l'entité passée en argument de la méthode `getRepository()` (ligne 30) (par défaut il s'agit de l'entité `Température`). Attention à bien importer l'entité, par exemple: `use App\Entity\Temperature;`.
Pour modifier la propriété de l'entité à afficher, modifier ligne 33 le getter appelé (par défaut `getTemperature()`).
Il est possible d'afficher plusieurs courbes sur le même graphique (voir `website\src\Twig\Components\RealTimeChartDHT.php`)
Il faut aussi créer une copie du fichier `website\templates\components\GenericChart.html.twig` et la nommer avec le même nom que le fichier .php créé précédemment.
Enfin, il faut intégrer le composant dans une page en utilisant `{{ component('NOM DU COMPOSANT') }}`