# Wordpress-Docker stack restoration

# INTRODUCTION
You have deployed your wordpress site using docker with docker-compose and you want to restore it on another machine
# REQUIREMENTS
- Operating system: Debian Bullseye
- System version : 64 bits
- Docker & Docker-compose
### BACKUP
- Access the machine as root (pc,rasperrypi..) in order to copy the folder located in /var/lib/docker where are the containers, images, volumes... 

- As well as the directory where are the files such as the docker-compose and other files necessary to the reconstruction of your stack.
# PROCESS
### DOCKER / DOCKER COMPOSE SETUP
- Install docker and docker-compose on your machine. 

For the installations see this link on my repot git : https://gitlab.com/Mxaxax/installations
### CONFIGURATION
After installing docker and docker-compose on your new machine :

- As root, move the docker folder located in /var/lib/ like this :
```
sudo mv /var/lib/docker var/lib/docker.BACK
```
Copy the contents of your backup to the directory of your new machine in var/lib/docker like this :
```
sudo cp /PATH/PATH/docker/ var/lib/docker/
```
## REBUILD
When you have done this, if you had not stopped the containers on your old machine they will be displayed 'up' otherwise you will have to rebuild your stack from the backed up volumes.

Very important, when you rebuild your stack it is very likely that you will encounter a problem related to the database when accessing your wordpress site. 

So you will have to stop and delete the containers and images, I advise you to use a docker-compose.yml available on the docker hub website : https://hub.docker.com/_/wordpress

When you have removed the containers and images, proceed as follows to rebuild your wordpress stack in the directory where your docker -compose is located :
```
docker-compose up -d
```

example : docker-compose.yml
```
version: '3.1'

services:

  wordpress:
    image: wordpress
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: exampleuser
      WORDPRESS_DB_PASSWORD: examplepass
      WORDPRESS_DB_NAME: exampledb
    volumes:
      - wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: exampledb
      MYSQL_USER: exampleuser
      MYSQL_PASSWORD: examplepass
      MYSQL_RANDOM_ROOT_PASSWORD: '1'
    volumes:
      - db:/var/lib/mysql

volumes:
  wordpress:
  db:
```
## PERMISSION
When you have rebuilt your stack from your initial docker-compose, you will encounter some permissions errors when connecting to your wordpress admin space. (This is due to the fact that the rights have been assigned to root.)

Therefore you need to enter your wordpress_wordpress container to assign permissions to : www-data

To do so, proceed as follows :

To enter the container
```
docker exec -it wordpress_wordpress_1
```
Assign permissions to www-data
```
chown -R www-data:www-data /var/www/html/
```
Exit the container

Restart the docker service 
```
sudo systemctl restart docker.service
```

