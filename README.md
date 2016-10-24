![nginx stable](https://img.shields.io/badge/nginx-stable-brightgreen.svg?style=plastic) ![php 7](https://img.shields.io/badge/php-7-yellowgreen.svg?style=plastic) ![mysql 5.6](https://img.shields.io/badge/mysql-5.6-yellow.svg?style=plastic) ![License MIT](https://img.shields.io/badge/license-MIT-blue.svg?style=plastic) 

Symfony-lemp7 sets up 3 containers running nginx, PHP7 and MySQL 5.6.

### Usage

To run it:

    $ docker-compose up --build -d


The containers expose :

    - Port 81 for HTTP
    - Port 3307 for MySQL


### To create

You have to create the folder www as DocumentRoot of your web application.


### Application configuration

To communicate with MySQL, you have to use the real host IP as hostname in the application configuration file.


### Docker Compose

```yaml
version: '2'

services:
  db:
    image: mysql:5.6
    container_name: symfony-mysql
    ports:
      - "3307:3306"
    environment:
      - "MYSQL_ROOT_PASSWORD=mysql"
      - "MYSQL_USER=user"
      - "MYSQL_PASSWORD=password"
      - "MYSQL_DATABASE=dbname" 
    volumes:
      - ./mysql-data:/var/lib/mysql:rw
      - ./mysql/conf.d/custom.cnf:/etc/mysql/conf.d/custom.cnf:ro
      - ./mysql/conf.d/my.cnf:/etc/mysql/my.cnf:ro
  php:
    build: ./php/
    container_name: symfony-php
    volumes:
        - "./www:/var/www/html:rw"
        - "./php/conf.d/php.ini:/usr/local/etc/php/conf.d/custom.ini:ro"
    links:
        - "db:db"
    working_dir: "/var/www/html"
  nginx:
    build: ./nginx/
    container_name: symfony-nginx
    ports:
      - "81:80"
    links:
      - "php:php"
    volumes:
      - "./www:/var/www/html:rw"
      - "./nginx/conf/default.conf:/etc/nginx/conf.d/default.conf:ro"
      - "./logs:/var/log/nginx:rw"
```
