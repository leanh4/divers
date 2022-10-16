# Prérequis
Portainer s'installe sur une VM sous forme de docker <p/>
Portainer consists of two elements, the Portainer Server, and the Portainer Agent. Both elements run as lightweight Docker containers on a Docker engine.
 
Pour installer on a besoin:
 - La dernière version de docker installée et fonctionnelle
 - Accès sudo sur la machine qui héberge l'instance  Portainer Server
 -  Par défaut Portainer expose IHM sur le port 9443 et un serveur tunnel TCP sur le port 8000. Le dernier est optionnel et seulement nécessaire si on veut que le serveur communique avec les agents


# Déploiement standard
 
First, create the volume that Portainer Server will use to store its database:<p/>
`docker volume create portainer_data`
 
Then, download and install the Portainer Server container:
```bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:latest
```
 
Now that the installation is complete, you can log into your Portainer Server instance by opening a web browser and going to:
https://localhost:9443

# Déploiement avec docker-compose

- Créer dans le répertoire `/products/potainer` un fichier [docker-compose.portainer.yml](./docker-compose.portainer.yml):
```yaml
# It is used to deploy the services on which the operation depends
version: '3.9'
services:
# Basic environment components
# Portainer
  portainer:
    image: portainer/portainer-ce
    container_name: portainer
    command: -H unix:///var/run/docker.sock
    restart: always
    deploy:
      resources:
        limits:
          cpus: '0.50'
          memory: 800M
        reservations:
          cpus: '0.1'
          memory: 256M
    ports:
      - "9443:9443"
      - "8000:8000"
    environment:
      TZ: Europe/Paris
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock # Data file mount
      - portainer_data:/data portainer/portainer-ce # Profile mount
#  Storage volume
volumes:
  portainer_data:
```
 
- On fait un lien symbolique vers le fichier [docker-compose.portainer.yml](./docker-compose.portainer.yml)
```bash
cd /products/potainer
ln -s docker-compose.portainer.yml  docker-compose.yml
```
## Démarrer portainer avec la commande
```bash
cd /products/potainer
docker-compose up -d
```
 
## L'accès se fait via  https://localhost:9443
 
## Arrêter portainer avec la commande à partir du répertoire 
```bash
cd /products/potainer
docker-compose stop
```
## Relancer portainer avec la commande à partir du répertoire 
```bash
cd /products/potainer
docker-compose start
```
