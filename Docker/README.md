## Installation de docker-compose
Docker compose V2 is integrated in the `docker` command: ` docker compose`.

If we want the `docker-compose` command, we need to install it with the below procedure

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

## Removing docker-compose
[How To Remove Docker Containers, Images and Volumes](https://shisho.dev/blog/posts/docker-remove-cheatsheet/)

Sometimes we want to delete the object created by docker-compose in order to clean the development environment and recreate it from scratch.<p/>
Below is an example of a command to clean all containers, images, volumes, networks, and undefined containers created with docker-compose.
```bash 
docker-compose down --rmi all -v --remove-orphans
```

`docker-compose down` is the exact opposite of `docker-compose up`. By default, it deletes created objects such as containers and networks. In addition, each option means the following.

- `-- rmi` all Remove all images 

- `-v` Remove the named volumes declared in the volumes section of docker-compose.yml and the anonymous volumes attached to the container

- `--remove-orphans` Remove containers not defined in `docker-compose.yml`

Therefore, this command example removes containers, images, volumes, networks, and undefined containers.
