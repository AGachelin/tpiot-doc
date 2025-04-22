---
sidebar_position: 1
---

# Synchronisation du temps
Le Réseau de l'Umons a tendance à bloquer le protocole NTP (Network Time Protocol). Si l'heure et la date sont trop déréglées, cela peut créer des erreurs du type `Release file for http://deb.debian.org/debian-security/dists/bookworm-security/InRelease is not valid yet (invalid for another 1h 38min 49s). Updates for this repository will not be applied.` Il faut donc régler l'heure du Raspberry Pi à la main (`sudo date -s 2025-04-14`  `sudo date -s 10:36`)