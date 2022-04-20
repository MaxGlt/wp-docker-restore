# Wordpress-Docker stack restoration

## Getting started
You have deployed your wordpress site using docker and you want to restore it on another machine

## Pocessus
###Access the machine (pc,rasperrypi..) in order to copy the folder located in /var/lib/docker where are the containers, images, volumes... 
###As well as the directory where are the files such as the docker-compose and other files necessary to the reconstruction of your stack.
###After installing docker and docker-compose on your new machine :
###As root, move the docker folder located in /var/lib/ like this
```
sudo mv /var/lib/docker var/lib/docker.BACK
```
###Copy the contents of your backup to the directory of your new machine in var/lib/docker 
## Integrate with your tools
