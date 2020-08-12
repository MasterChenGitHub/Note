# install docker on ubuntu
https://docs.docker.com/engine/install/ubuntu/

# install guacamole
http://guacamole.apache.org/doc/gug/guacamole-docker.html

## summary:
docker pull guacamole/guacd
docker pull guacamole/guacamole
docker pull mysql/mysql-server

# initdb.sql
docker run --rm guacamole/guacamole /opt/guacamole/bin/initdb.sh --mysql > initdb.sql


# install mysql
docker run --name remote-desk-mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_ONETIME_PASSWORD=yes -d mysql/mysql-server

docker cp initdb.sql remote-desk-mysql:/initdb.sql

docker logs mysql
//extract generated password
docker logs mysql 2>&1 | grep GENERATED

docker exec -it remote-desk-mysql bash

ALTER USER 'root'@'localhost' IDENTIFIED BY 'password';

CREATE DATABASE guacamole;
CREATE USER 'guacamole'@'%' IDENTIFIED BY 'guacamole';
GRANT SELECT,INSERT,UPDATE,DELETE ON guacamole.* TO 'guacamole'@'%';
FLUSH PRIVILEGES;

cat initdb.sql | mysql -u root -p guacamole

# start guacd
docker run --name boya-guacd -d guacamole/guacd

# link guacd to guacamole and mysql
docker run --name boya-guacamole --link boya-guacd:guacd --link remote-desk-mysql:mysql -e MYSQL_DATABASE='guacamole' -e MYSQL_USER='guacamole' -e MYSQL_PASSWORD='guacamole' -d -p 8080:8080 guacamole/guacamole
# test 
http://server IP:8080/guacamole/
-- Create default user "guacadmin" with password "guacadmin"

# config vnc
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-18-04
# FAQ
## vscode 无法启动
sed -i 's/BIG-REQUESTS/_IG-REQUESTS/' /usr/lib/x86_64-linux-gnu/libxcb.so.1

## terminal无法打开
update-alternatives --config x-terminal-emulator
选择xfce

## 开放端口
firewall-cmd --add-port=8080/tcp --permanent
firewall-cmd --add-port=3306/tcp --permanent
firewall-cmd --reload






