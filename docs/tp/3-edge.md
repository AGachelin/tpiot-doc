---
sidebar_position: 3
---

# Edge
1. Créer une copie du fichier `credentials.h` dans les dossier se terminant par `.ino`
2. Entrer le SSID et le mot de passe du Wi-Fi à utiliser dans tous les fichiers `credentials.h` créés.
3. Modifier l'ip dans les fichiers `conf.h` pour qu'elle corresponde à l'ip de la machine fog (voir [ici](../Autres/ip_discovery.md)).
4. Dans l'IDE Arduino, installer les extensions `DHT sensor library` par Adafruit, `ArduinoJson` par Benoit Blanchon et `PubSubClient` par Nick O'Leary.
5. Avec l'IDE Arduino ouvrir le dossier .ino et transférer le code sur la carte esp32.