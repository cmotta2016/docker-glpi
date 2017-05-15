![alternate text](https://raw.githubusercontent.com/glpi-project/glpi/master/pics/logos/logo-GLPI-250-black.png)
# What's GLPI?
GLPI is the Information Resource-Manager with an additional Administration- Interface. You can use it to build up a database with an inventory for your company (computer, software, printers...). It has enhanced functions to make the daily life for the administrators easier, like a job-tracking-system with mail-notification and methods to build a database with basic information about your network-topology.

# About image
There is two versions of glpi: GLPI version: 9.1.2 (latest) and 9.1.1 (cmotta2016/glpi:9.1.1)
PHP version: 2.6.29
Web Server: Apache2.4.10
OS:Debian Jessie

# How to use this image
You will need a database. The following example run container in background linking to a database container.
```
# docker run -d -p 80:80 --link some-mysql cmotta2016/glpi
```
# Persistent data
To make data persistent, export the following volume:
```
# docker run -d -p 80:80 --link some-mysql -v /some/host/dir/:/var/www/ cmotta2016/glpi
```
# Docker Compose
Deploy both glpi and mysql with compose.
First create a dir and file called docker-compose.yml with following content:
```
version: '2'

services:
   db:
     image: mysql
     volumes:
       - /opt/database:/var/lib/mysql
     restart: unless-stopped
     environment:
       MYSQL_ROOT_PASSWORD: glpi
     container_name: database

   glpi:
     depends_on:
       - db
     image: cmotta2016/glpi
     ports:
       - "80"
     restart: unless-stopped
     volumes:
       - /opt/systems:/var/www
     container_name: glpi-app
```

So, inside work dir, run the following command and enjoy it:
```
# docker-compose up -d
```
