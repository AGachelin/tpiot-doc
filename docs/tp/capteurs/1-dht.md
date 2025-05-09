---
sidebar_position: 1
---

# DHT 22

Le capteur DHT 22 est un capteur combiné température-humidité avec une interface numérique.
La communication avec le capteur est implémentée par la librairie `DHT sensor library` par Adafruit.
Pour l'utiliser : 
```c
#include <DHT.h> //Importatiton de la librairie.
DHT dht(DHTPIN, DHTTYPE) //Initialisation de l'objet permettant la communication.
dht.begin() //Initialisation de la communication.
float h = dht.readHumidity(); //Lecture de l'humidité.
float t = dht.readTemperature(); //Lecture de la température.
```