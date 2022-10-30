# How can I add hostnames to a container on the same docker network? #
https://stackoverflow.com/questions/33695702/how-can-i-add-hostnames-to-a-container-on-the-same-docker-network#comment98967056_39707966

## Alias
In a docker compose file, one can specify [network aliases](https://docs.docker.com/compose/compose-file/#aliases).

``` yml
services:
  db:
    networks:
      default:
        aliases:
          - database
          - postgres
```          
In this example, the `db` service could be reached by other containers on the default network using `db`, `database`, or `postgres`.

You can also add aliases to running containers using the docker network connect command with the --alias= option.

The closest option  is to still disconnect container from the network and add it again with alias.

**Commands:**
``` bash
docker network list 
docker network disconnect dns-alias-network my-centos-container
docker network connect --alias my-centos-alias dns-alias-network my-centos-container . 
``` 
Any options to avoid disconnecting container? - Error response from daemon: endpoint with name my-centos already exists in network dns-alias-network

## Extra hosts
Docker compose has an [`extra_hosts`](https://docs.docker.com/compose/compose-file/#extra-hosts) feature that allows additional entries to be added to the container's host file.
### Example
**docker-compose.yml**
``` yml
web1:
  image: tomcat:8.0
  ports:
    - 8081:8080
  extra_hosts:
    - "somehost:162.242.195.82"
    - "otherhost:50.31.209.229"
web2:
  image: tomcat:8.0
  ports:
    - 8082:8080
web3:
  image: tomcat:8.0
  ports:
    - 8083:8080
```    

Run docker compose with the new docker 1.9 networking feature:

``` bash
$ docker-compose --x-networking up -d
Starting tmp_web1_1
Starting tmp_web2_1
Starting tmp_web3_1
```
and look at the hosts file in the first container. Shows the other containers, plus the additional custom entries:
``` bash
$ docker exec tmp_web1_1 cat /etc/hosts 
..
172.18.0.4  web1
172.18.0.2  tmp_web2_1
172.18.0.3  tmp_web3_1
50.31.209.229   otherhost
162.242.195.82  somehost
```
