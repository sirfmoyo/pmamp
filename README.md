# PMAMP - PhpMyadmin with Apache Mysql and Php
- Docker with Apache, PHP, MySQL, phpMyAdmin

## Prerequisites
- Install and run Docker Desktop
  - [https://www.docker.com/get-started ](https://www.docker.com/get-started)

## Run Docker images
On the command line (the terminal)
- Clone this repository where you want it.
  - `git clone `
- Change into the directory
- `cd pmamp`
- Change the MySQL account info in the `docker-compose.yml` file if you want
 
```
  MYSQL_ROOT_PASSWORD: "rootPASS"
  MYSQL_DATABASE: "dbase"
  MYSQL_USER: "dbuser"
  MYSQL_PASSWORD: "dbpass"
```
- Start the container
  - `docker-compose up`
  - Or run it in the background to free up the terminal
    - `docker-compose up -d`
- To stop the containers
  - press ctrl-c
  - then run `docker-compose down`
- View the web pages at [http://localhost ](http://localhost)
- View phpMyAdmin at [http://localhost:8080 ](http://localhost:8080)
  - type in the db user name and db password to log in

## Info 
- This will run three containers: a PHP-Apache container, a MySQL container and
a phpMyAdmin container.
- All of the files for the website building can go in the `public_html` folder.
- The database files are stored in the `dbdata` folder. This allows for the
  data to persist between restarts and for hands on access.
  - To restart with a clean database, just delete this folder.
  - To seed the database with a database, tables, and data, just uncomment the
    line in the docker-compose.yml file referencing `seed.sql`. The `dbdata`
    folder will need to be deleted first. This works best if using a mysql dump
    file. Otherwise, the sql file just needs to have valid SQL statments.
    - `#- ./seed.sql:/docker-entrypoint-initdb.d/database.sql`


# Use a real domain name
It's relatively easy to use this setup with a real domain name for better
testing and without the need of ports.

## Requirements
- All of the requirements as above, but an additional image needs to run and
  using a different docker-compose.yml file. See steps below.
- Create a new docker network
  - `docker network create traefikNetwork`

## Run Traefik
To get this to work easily, we'll use the Traefik image from here: https://hub.docker.com/_/traefik/
- Documentation is here: https://doc.traefik.io/traefik/

You will run this container just as you did the one for serving PMAMP.
- Change into the 'traefik' directory
  - `cd traefik`
- Start the container
  - `docker-compose up -d`

To use the domain name version, start the container with the specified compose
file.
- While in the terminal and in the 'pmamp' directory, run the command:
  - `docker-compose -f docker-compose-traefik.yml up -d`

lvh.me is a free service that redirects to localhost, so now you can access the
site at http://lvh.me instead of http://localhost

phpMyAdmin is now accessible at http://pma.lvh.me

## Notes
You can have multiple domains and subdomains pointing to a single container
using the Hosts line in the label section of docker-compose-traefik.yml
    - "traefik.http.routers.php-apache.rule=Host(`lvh.me`, `fun.lvh.me`, `realdomain.com`)"
