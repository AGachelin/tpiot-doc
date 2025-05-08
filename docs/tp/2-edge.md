---
sidebar_position: 2
---
# Programmation de l'ESP 32

## Structure de fichiers
Pour que le programme de l'ESP 32 fonctionne correctement, il faut créer un dossier qui contiendra le code. Ce dossier doit avoir la structure suivante:
```c
fog
└──myFile
    ├──conf.h //Fichier contenant la configuration réseau
    ├──credentials.h //Fichier contenant les identifiants Wi-Fi
    ├──edge.cpp //Fichiers contnant les fonctions fournies 
    ├──edge.h
    └──myCode.ino //Votre Code
```
Le fichier `credentials.h` doit contenir le SSID et le mot de passe du réseau Wi-Fi.
Le fichier `conf.h` doit contenir l'ip du serveur vers lequel on va publier.
Les fichiers `edge.h` et `edge.cpp` contiennent des fonction utiles.

Le fichier contenant votre code doit commencer par : 
```c
#include <WiFi.h>
#include <PubSubClient.h>
#include <ArduinoJson.h>
#include <DHT.h>
#include <time.h>
#include <stdint.h>
#include "credentials.h"
#include "conf.h"
#include "edge.h"

#define DHTPIN 21     // Digital pin connected to the DHT sensor
#define DHTTYPE DHT22 // DHT 22 (AM2302)

DHT dht(DHTPIN, DHTTYPE);
WiFiClient espClient;
PubSubClient client(espClient);

uint64_t T0NTP;
uint64_t T0Milis;

// NTP config
const char *ntpServer = "pool.ntp.org";
const long gmtOffset_sec = 3600;     // Adjust for your timezone (e.g., -3600 for GMT-1)
const int daylightOffset_sec = 3600; // Adjust for daylight saving time
```

La fonction `setup` doit impérativement appeler les méthodes suivantes:
- `Serial.begin(115200)` : Commence la communication via port série avec l'ordinateur.
- `setup_wifi(ssid, password)` : Paramètre et connecte le Wi-Fi.
- `client.setServer(mqtt_server, 1883)` : Paramètre le serveur MQTT.
- `setupTime()` : Effectue une synchronisation NTP et si elle échoue demande à l'utilisateur d'entrer le timestamp UNIX à la main.
- `dht.begin()` : Commence la communication avec le capteur DHT.

La fonction loop doit appeler les méthodes:
- `client.loop()` : Maintient la connexion MQTT ouverte et traite les messages.

## Fonctions Fournies 

```c
void setup_wifi(const char *ssid, const char *password); //Initialisation du Wi-Fi
void reconnect_Mqtt(); //Connexion ou reconnexion à MQTT
void setupTime(); //Initialisation du temps
uint64_t unixMilis(); //Donne le timestamp UNIX actuel en milisecondes
uint64_t parseUint64(String s); //transforme une chaîne de caractères en uint64_t
```
Pour initier la connexion, il faut appeler `reconnect_Mqtt()` au moins une fois mais il est préférable de tester l'était de la connexion et de se reconnercter si besoin. La méthode `client.connected()` renvoie l'état de la connexion MQTT sous forme de booléen. \
Les méthodes `dht.readHumidity()` et `dht.readTemperature()` renvoient respectivement l'humidité et la température sous forme de booléen. \
La méthode `unixMilis()` renvoie le timestamp UNIX en millisecondes, c'est cette valeur qu'il faut utiliser dans kafka et MQTT. 

## Structure des messages

Les messages envoyés au cloud doivent être sous la forme ci-après. Un objet JSON avec deux champs : `schema` et `payload`. \
Le champ `schema` contient la description du champ `payload` : dans notre cas, le type, un `struct` et les champs avec leurs type, si ils sont optionnels et leur nom. \
Le champ `payload` contient les attributs décrits dans le champ `schema`. Attention à la casse sur les noms. \
Les messages peuvent contenir un nombre arbitraire de champs
```json title="Exemple d'un message valide"
{
  "schema": {
    "type": "struct",
    "fields": [
      {
        "type": "int",
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
    "temperature": 24,
    "timestamp": 1745943209505
  }
}
```
Pour construire des objets JSON facilement, on utilise la librairie `<ArduinoJson.h>`.
```c title="Code utilisé pour l'exemple"
JsonDocument jsonDocTemp;
jsonDocTemp["schema"] = JsonObject();
jsonDocTemp["schema"]["type"] = "struct";
jsonDocTemp["schema"]["fields"] = JsonArray();
JsonObject tempField = jsonDocTemp["schema"]["fields"].createNestedObject();
tempField["type"] = "int";
tempField["optional"] = false;
tempField["field"] = "temperature";
JsonObject timeFieldTemp = jsonDocTemp["schema"]["fields"].createNestedObject();
timeFieldTemp["type"] = "int64";
timeFieldTemp["optional"] = false;
timeFieldTemp["field"] = "timestamp";
jsonDocTemp["payload"] = JsonObject();
jsonDocTemp["payload"]["temperature"] = t;
jsonDocTemp["payload"]["timestamp"] = timestamp;

char jsonBufferTemp[400];
serializeJson(jsonDocTemp, jsonBufferTemp);
```

Le nom du topic MQTT doit aussi être un objet JSON (ex : `{"temperature":"string"}`)