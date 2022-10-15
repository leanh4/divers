## Installation de docker-compose
Docker compose V2 est intégré dans la commande docker : ` docker compose`

La procédure pour installer la commande ` docker-compose` :
```bash
# Get the latest version of docker-compose
curl -s https://api.github.com/repos/docker/compose/releases/latest | grep browser_download_url  | grep docker-compose-linux-x86_64 | cut -d '"' -f 4 | wget -qi -
chmod +x docker-compose-linux-x86_64
sudo mv docker-compose-linux-x86_64 /usr/local/bin/docker-compose
docker-compose --version

sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo cp /usr/local/bin/docker-compose /usr/local/lib/docker/cli-plugins


# Bash completion
sudo mkdir -p /etc/bash_completion.d
sudo curl -L https://raw.githubusercontent.com/docker/compose/master/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
```

