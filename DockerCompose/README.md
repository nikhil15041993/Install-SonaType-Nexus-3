# Install SonaType Nexus 3 using Docker Compose

## Pre-requistes:

* Ubuntu EC2 up and running with at least t2.medium(4GB RAM), 2GB will not work
* Port 8081, 8085 is opened in security firewall rule
* instance should have docker and docker-compose installed

## Perform System update
sudo apt-get update

## Install Docker
sudo apt-get install docker -y

## Install Docker-Compose
sudo apt-get install docker-compose -y

## Add current user to Docker group
sudo usermod -aG docker $USER

## Create docker-compose.yml
this yml has all the configuration for installing Nexus on Ubuntu EC2.
sudo vi docker-compose.yml 

```
version: "3"
services:
  nexus:
    image: sonatype/nexus3
    restart: always
    volumes:
      - "nexus-data:/sonatype-work"
    ports:
      - "8081:8081"
      - "8085:8085"

volumes:
  nexus-data: {}
  
```
Now execute the compose file using Docker compose command to start Nexus Container
```
sudo docker-compose up -d 
```

-d means detached mode

Make sure Nexus 3 is up and running
```
sudo docker-compose logs --follow
```


We need to login to docker container to get admin password.
Identify Docker container name
sudo docker ps


Get admin password by executing below command
```
sudo docker exec -it ubuntu_nexus_1 cat /nexus-data/admin.password
```
