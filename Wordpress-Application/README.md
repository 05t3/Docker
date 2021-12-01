# Running Wordpress on Docker

`sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`sudo chmod +x /usr/local/bin/docker-compose`

```bash
root@docker:/var/www/wordpress# docker-compose --version
docker-compose version 1.29.2, build 5becea4c
```

```bash
docker@docker:~/Desktop$ cd /var/www/
docker@docker:/var/www$ ls -la
total 12
drwxr-xr-x  3 root root 4096 Onk 18 22:57 .
drwxr-xr-x 15 root root 4096 Onk 18 22:57 ..
drwxr-xr-x  2 root root 4096 Onk 18 22:57 html
docker@docker:/var/www$ sudo su
[sudo] password for docker:
root@docker:/var/www# mkdir wordpress
root@docker:/var/www# cd wordpress/
root@docker:/var/www/wordpress# nano docker-file.yml
root@docker:/var/www/wordpress# cat docker-file.yml
version: "3.9"

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
  wordpress_data: {}
```

You can then proceed to build the project as follows:

`docker-compose up -d`

The command above basically runs in a detached mode, oulls the needed Docker Images from the docker registry and starts the wordpress and database containers.

```bash
root@docker:/var/www/wordpress# docker-compose up -d
Creating network "wordpress_default" with the default driver
Creating volume "wordpress_db_data" with default driver
Creating volume "wordpress_wordpress_data" with default driver
Pulling db (mysql:5.7)...
5.7: Pulling from library/mysql
a10c77af2613: Pull complete
b76a7eb51ffd: Pull complete
258223f927e4: Pull complete
2d2c75386df9: Pull complete
63e92e4046c9: Pull complete
f5845c731544: Pull complete
bd0401123a9b: Pull complete
2724b2da64fd: Pull complete
d10a7e9e325c: Pull complete
1c5fd9c3683d: Pull complete
2e35f83a12e9: Pull complete
Digest: sha256:7a3a7b7a29e6fbff433c339fc52245435fa2c308586481f2f92ab1df239d6a29
Status: Downloaded newer image for mysql:5.7
Pulling wordpress (wordpress:latest)...
latest: Pulling from library/wordpress
eff15d958d66: Pull complete
933427dc39f7: Pull complete
35bb08dc7ee2: Pull complete
58a3f26800d7: Pull complete
bba5c0e912ac: Pull complete
62e5c0d120c5: Pull complete
410a26230a3b: Pull complete
2880852a4de4: Pull complete
ea5136838c9b: Pull complete
e11691a8fc82: Pull complete
d85eae320cca: Pull complete
7fb6fb87e009: Pull complete
f47ba3377e25: Pull complete
b768e2589a07: Pull complete
27d9df1ded76: Pull complete
be48c3fa7409: Pull complete
ccb83b81b398: Pull complete
33d5041f0578: Pull complete
d1674b6f1114: Pull complete
65c90de6fe6b: Pull complete
69fce6e644d9: Pull complete
Digest: sha256:e7b40265f5d72fc74fb73683a60f5d3de9787a6467b87b78ee742645807d8463
Status: Downloaded newer image for wordpress:latest
Creating wordpress_db_1 ... done
Creating wordpress_wordpress_1 ... done
```

```bash
root@docker:/var/www/wordpress# docker images
REPOSITORY                                      TAG            IMAGE ID       CREATED         SIZE
wordpress                                       latest         e3a452c0a154   13 days ago     616MB
mysql                                           5.7            8b43c6af2ad0   2 weeks ago     448MB
```

```bash
root@docker:/var/www/wordpress# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED         STATUS          PORTS                                       NAMES
fbeb6f8b9e6f   wordpress:latest             "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes    0.0.0.0:8000->80/tcp, :::8000->80/tcp       wordpress_wordpress_1
cc8a525a6502   mysql:5.7                    "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes    3306/tcp, 33060/tcp                         wordpress_db_1

```

`docker-compose down`

```bash
root@docker:/var/www/wordpress# docker-compose down
Stopping wordpress_wordpress_1 ... done
Stopping wordpress_db_1        ... done
Removing wordpress_wordpress_1 ... done
Removing wordpress_db_1        ... done
Removing network wordpress_default
root@docker:/var/www/wordpress# docker ps
CONTAINER ID   IMAGE                        COMMAND                  CREATED       STATUS          PORTS

root@docker:/var/www/wordpress#
```
