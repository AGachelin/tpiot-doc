# Installer Git
Git est un système de gestion de version décentralisé  qui permet de suivre les modifications dans des projets de développement logiciel. 

## Installation

### Linux (Ubuntu/Debian)
```
sudo apt update && sudo apt upgrade
sudo apt install git
```

### Windows

#### Avec winget

Winget est un gestionnaire de paquets intégré à Windows.

`winget install --id Git.Git -e --source winget` 

#### Sans winget

Si votre ordinateur n'est pas compatible avec winget, rendez-vous sur la page officielle de téléchargement de Git pour Windows ([Télécharger Git pour Windows](https://git-scm.com/downloads/win)) puis télécharger le fichier correspondant à votre architecture.

Une fois le téléchargement terminé, lancez le fichier .exe pour démarrer l'installation.

Paramètres de PATH : Il est recommandé de choisir l'option par défaut qui vous permet d'ajouter Git au PATH, ce qui facilite l'utilisation de Git en ligne de commande.

Cliquez sur "Install" pour finaliser l'installation.

## Vérification de l'installation 

Executer la commande `git --version`. Si Git est installé correctement, vous verrez un message indiquant la version de Git.

Avant de commencer à utiliser Git, il est recommandé de configurer votre nom d'utilisateur et votre email, car ces informations seront utilisées pour les commits.

Dans votre terminal, entrez les commandes suivantes :

```
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@example.com"
```