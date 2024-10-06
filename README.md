# Introduction

This is a Docker compose sample implemented in Ubuntu server where are enabled the following services:

* Nginx
* Php
* Mariadb

## Pre-Requisites

1. Ubuntu server in a VM or in a physical device.
2. Install Docker: https://docs.docker.com/engine/install/
3. Install Docker compose: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04
4. Install MariaDb: https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04
5. Install Ngnix: https://ubuntu.com/server/docs/how-to-install-nginx

# How to use the code

1. Clone the project in your local device 

```
sudo git clone git@github.com:itzellRangel/test.git
```

## User interface

1. In php_code folder clone the git project used as user interface

```
sudo git clone https://github.com/rapidcode-technologies-private-limited/php-e-commerce.git ~/docker-project/php_code/
```

## MariaDB config

1. In test folder create a CLI session of MariaDB container 

```
sudo docker exec -it test_db_1 /bin/sh
```

2. Access MariaDB as root user

```
mariadb -u root -pmariadb
```

3. Create new user for db

```
CREATE USER 'thiio'@'%' IDENTIFIED BY "thiio123";
```

4. Grant privileges to the new db user

```
GRANT ALL PRIVILEGES ON *.* TO 'thiio'@'%';
FLUSH PRIVILEGES;
exit
```

5. Information addition into db to use the user interface

```
cat > db-load-script.sql <<-EOF
USE ecomdb;
CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;

INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");

EOF
```

6. Run the db script

```
mariadb -u root -pmariadb < db-load-script.sql
exit
```

7. In php_code folder open index.php (line 106) file to change the user information to db connection

```
.
.
.
 $link = mysqli_connect('db', 'thiio', 'thiio123', 'ecomdb');
.
.
.
```

## Executing Docker compose

2. In test folder launch docker compose

```
sudo docker-compose up -d
```

3. Check the enabled services list

```
sudo docker-compose ps
```

4. Open the web browser and access to Ngnix using the default URL

```
http://your-server-ip || http://localhost
```

5. Turn off docker compose every time when any change is made

```
sudo docker-compose down
```
