# Installer Docker
## Linux (Ubuntu/Debian)
Docker est un outil qui permet d'exécuter des applications dans des **conteneurs**, des environnements isolés et légers. Cela garantit que l'application fonctionne de la même manière sur n'importe quel ordinateur, sans conflits avec le système.
1. Retirer les paquets créant des conflits:
```
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
2. Ajouter le dépot apt de Docker
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
$(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Si cette étape échoue, se référer à [cette page](../troubleshooting/time.md).

3. Installer Docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
4. Il faut ensuite rejoindre le groupe des utilisateurs autorisés à utiliser docker
```
sudo usermod -aG docker $USER
```
## Windows
### WSL 

WSL (_Windows Subsystem for Linux_) est une fonctionnalité de Windows qui permet d'exécuter un environnement Linux directement sur Windows, sans avoir besoin d'une machine virtuelle ou d'un dual boot. Pour l'installer :
```
wsl --install
```

### Docker Desktop

Docker Desktop est une application qui permet d'exécuter et de gérer des conteneurs Docker sur Windows et macOS via une interface simplifiée.
 1. Télécharger et exécuter l'installer https://docs.docker.com/desktop/setup/install/windows-install/
 2. Lancer Docker Desktop
 3. Dans Settings > General cocher la case `Use the WSL 2 based engine`