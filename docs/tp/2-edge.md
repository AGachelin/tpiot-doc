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

## Fonctions Fournies 

```c
void setup_wifi(const char *ssid, const char *password); //Initialisation du Wi-Fi
void reconnect_Mqtt(); //Connexion ou reconnexion à MQTT
void setupTime(); //Initialisation du temps
uint64_t unixMilis(); //Donne le timestamp UNIX actuel en milisecondes
uint64_t parseUint64(String s); //transforme une chaîne de caractères en uint64_t
```