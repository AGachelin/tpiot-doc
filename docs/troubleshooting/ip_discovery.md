---
sidebar_position: 2
---

# Obtention de l'ip d'une machine sur le même réseau
## Si le raspberry est connecté sur le partage de connexion de l'ordinateur

Dans cette situation, il suffit en général d'aller dans les paramètres du partage de connexion de l'ordinateur. Les appareils connectés sont affichés. Si le raspberry pi n'apparaît pas c'est qu'il y a eu un problème lors de l'installation de l'os (probablement en lien avec les identifiants) ou qu'il y a un problème avec le partage de connexion.
Si le raspberry pi apparait mais qu'il n'est pas possible de récupérer l'ip de cette manière, se référer à la section suivante.

## Si le raspberry n'est pas connecté sur le partage de connexion de l'ordinateur

1. Installer nmap
    - Sous windows :
    Télécharger et exécuter l'installer (nmap-X.XX-setup.exe) : [https://nmap.org/download.html#windows](https://nmap.org/download.html#windows)
    - Sous linux :
    `sudo apt-get install nmap`
2. Lancer un scan du réseau :  
`nmap -sn 192.168.X.0/24`  
Où X correspond au troisième nombre de l'addresse ip du cloud.
3. Noter les addresses ip récupérées de cette manière. Si l'addresse ip correspondant au raspberry est indiquée il est inutile de faire les étapes 4 et 5.
4. Eteindre le raspberry et réitérer les étapes 2 et 3.
5. Comparer les résultats obtenus en 2 et en 4. L'addresse ip du raspberry est normalement la seule qui était présente en 2 mais qu ne l'est plus en 4.