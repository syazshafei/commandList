# List of Useful Command

## Linux Management

Create user `admin` and add to sudoers

```
sudo adduser admin
sudo usermod -aG sudo admin
```

Kill port/service on Linux machine

```
sudo netstat -nltp|grep LISTEN
sudo netstat -nltp|grep :8080
sudo kill -TERM xxxxx
```

Other netstat command

```
sudo netstat -nputw
sudo netstat -tulpn
```

Example to kill java process

```
killall -9 java
```

List down systemctl services

```
systemctl --type=service
```

Follow systemctl logs

```
journalctl -f -u service_name
```

Copy file from a remote host to local host SCP example

```
scp username@from_host:file.txt /local/directory/
```

Copy file from local host to a remote host SCP example

```
scp file.txt username@to_host:/remote/directory/
```

Check local disk space

```
df -h
```

Check directory on which partition

```
df -h .
```

List all files with human readable file size and sorted by latest timestamp

```
ls -laht
```

Get size of all the directories

```
du -h --max-depth=1
```

Display biggest directories in the current working directory

```
du -hs * | sort -rh | head -5
```

Display the largest folders/files including the sub-directories

```
du -Sh | sort -rh | head -5
```

Find file recusively

```
find -L . -name "historical*"
```

Find file with `.tar.gz` extension in current directory and delete it

```
find . -name "*.tar.gz" -type f -delete
```

Find file with `.tar.gz` extension in current directory that older than 30 days and delete it

```
find . -name "*.tar.gz" -type f -mtime +30 -delete
```

Rename file extension for multiple files. Example: `.tar.gz` to `.tgz`

```
rename .tar.gz .tgz *.tar.gz
```

Vim configuration

```
vim ~/.vimrc
:set cursorline
:set hlsearch
:set incsearch
:set ignorecase
:set autoindent
:syntax on
```

Uninstall package

```
sudo apt list --installed
sudo apt purge package_name
```

## Docker

Install Docker CE 19.03 & Docker Compose on Ubuntu 18.04

```
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce=5:19.03.13~3-0~ubuntu-bionic docker-ce-cli=5:19.03.13~3-0~ubuntu-bionic containerd.io=1.3.7-1
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Install Docker CE 19.03 & Docker Compose on CentOS 7

```
sudo yum check-update
sudo yum install yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce-19.03.13 docker-ce-cli-19.03.13 containerd.io-1.3.7
sudo systemctl start docker
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Add docker to `admin` group

```
sudo usermod -aG docker admin
```

List docker container

```
docker ps
```

Run a command in a running container

```
docker exec -ti <container> bash
```

Copy files/folders between a container and the local filesystem

```
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Example:
docker cp /home/syazwan/2015.csv e76ae9f4fca8:/druid/druid-0.12.3/data
```

Export a container’s filesystem as a tar archive

```
docker export [OPTIONS] CONTAINER
```

Stop one or more running containers

```
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

Stop all containers

```
docker kill $(docker ps -q)
```

Remove all stopped containers

```
docker rm $(docker ps -a -q)
```

Remove docker image

```
docker rmi [IMAGE ID/IMAGE NAME]
```

Remove all docker images

```
docker rmi $(docker images -q)
```

Save one or more images to a tar archive (streamed to STDOUT by default)

```
docker save [IMAGE] > [PATH]
```

Load an image from a tar archive or STDIN

```
docker load < [IMAGE_PATH]
```

Display amount of disk space used by Docker:

```
docker system df -v
```

Remove all dangling images `<none>`

```
docker rmi $(docker images | grep "^<none>" | awk '{print $3}')
```

Follow docker container log

```
docker logs -f container
```

Follow docker compose logs

```
docker-compose logs -f
```

Check docker performance stats

```
docker stats
```

Check Docker container which is `exited`

```
docker ps -a -f status=exited
```

Remove Docker container which is `exited`

```
docker rm $(sudo docker ps -a -f status=exited -q)
```

Uninstall Docker and Docker Compose

```
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
sudo rm -rf /usr/local/bin/docker-compose
```

### Firewall
This solution needs to modify only one UFW configuration file, all Docker configurations and options remain the default.

Make backup to `after.rules` file first
```
sudo cp /etc/ufw/after.rules /etc/ufw/after.rules_backup
```

Modify the UFW configuration file `/etc/ufw/after.rules` and add the following rules at the end of the file:

    # BEGIN UFW AND DOCKER
    *filter
    :ufw-user-forward - [0:0]
    :ufw-docker-logging-deny - [0:0]
    :DOCKER-USER - [0:0]
    -A DOCKER-USER -j ufw-user-forward

    -A DOCKER-USER -j RETURN -s 10.0.0.0/8
    -A DOCKER-USER -j RETURN -s 172.16.0.0/12
    -A DOCKER-USER -j RETURN -s 192.168.0.0/16

    -A DOCKER-USER -p udp -m udp --sport 53 --dport 1024:65535 -j RETURN

    -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 192.168.0.0/16
    -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 10.0.0.0/8
    -A DOCKER-USER -j ufw-docker-logging-deny -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 172.16.0.0/12
    -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 192.168.0.0/16
    -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 10.0.0.0/8
    -A DOCKER-USER -j ufw-docker-logging-deny -p udp -m udp --dport 0:32767 -d 172.16.0.0/12

    -A DOCKER-USER -j RETURN

    -A ufw-docker-logging-deny -m limit --limit 3/min --limit-burst 10 -j LOG --log-prefix "[UFW DOCKER BLOCK] "
    -A ufw-docker-logging-deny -j DROP

    COMMIT
    # END UFW AND DOCKER

Using command `sudo systemctl restart ufw` or `sudo ufw reload` to restart UFW after changing the file. Now the public network can't access any published docker ports, the container and the private network can visit each other normally, and the containers can also access the external network from inside. **There may be some unknown reasons cause the UFW rules will not take effect after restart UFW, please reboot servers.**

If you want to allow public networks to access the services provided by the Docker container, for example, the service port of a container is `80`. Run the following command to allow the public networks to access this service:

    ufw route allow proto tcp from any to any port 80

This allows the public network to access all published ports whose container port is `80`.

Note: If we publish a port by using option `-p 8080:80`, we should use the container port `80`, not the host port `8080`.

If there are multiple containers with a service port of `80`, but we only want the external network to access a certain container. For example, if the private address of the container is `172.17.0.2`, use the following command:

    ufw route allow proto tcp from any to 172.17.0.2 port 80

If the network protocol of a service is UDP, for example a DNS service, you can use the following command to allow the external network to access all published DNS services:

    ufw route allow proto udp from any to any port 53

Similarly, if only for a specific container, such as IP address `172.17.0.2`:

    ufw route allow proto udp from any to 172.17.0.2 port 53

## PostgreSQL

Install postgresql

```
sudo apt-get install postgresql postgresql-contrib
```

Setting up password for `postgres` user

```
sudo -u postgres psql
ALTER USER postgres WITH ENCRYPTED PASSWORD 'xxxxxxx';
```

Connect to postgres user on postgreSQL

```
psql -h localhost -U postgres
```

Backup single db with YYYY-MM-DD format

```
pg_dump -U USERNAME > $(date +%Y-%m-%d).sql
```

Backup all db with YYYY-MM-DD format

```
pg_dumpall -U USERNAME > $(date +%Y-%m-%d).sql
```

Restore db

```
psql -U USERNAME DB_NAME < DB.sql
```

## Anaconda Environment

Create environemnt (full package)

```
conda create -n yourenvname python=x.x anaconda
```

Create environemnt (normal package)

```
conda create -n yourenvname python=x.x
```

Delete environment

```
conda remove --name yourenvname --all
```

## Superset Installation

To build docker image

```
docker-compose build
```

To run the container, simply run:
```
docker-compose up -d
```


## Druid

Ingest data into Druid (without console log)

```
curl -X 'POST' -H 'Content-Type:application/json' -d @quickstart/tutorial/wikipedia-index.json http://localhost:8081/druid/indexer/v1/task
```

Ingest data into Druid (with console log)

```
bin/post-index-task --file quickstart/tutorial/wikipedia-index.json --url http://localhost:8081
```

Query

```
curl -X 'POST' -H 'Content-Type:application/json' -d @quickstart/tutorial/wikipedia-top-pages-sql.json http://localhost:8888/druid/v2/sql
```

## SFTP Server

### Installation

```
sudo groupadd shared
sudo nano /etc/ssh/sshd_config
 Match User shared
 ForceCommand internal-sftp
 PasswordAuthentication yes
 PermitTunnel no
 AllowAgentForwarding no
 AllowTcpForwarding no
 X11Forwarding no
sudo service ssh restart
sudo useradd -m shared -g shared -s /usr/sbin/nologin
sudo passwd shared
sudo mkdir /home/shared/uploads
sudo mkdir /home/shared/data
sudo mkdir /home/shared/test
sudo chown shared:shared /home/shared/uploads
sudo chown shared:shared /home/shared/data
sudo chown shared:shared /home/shared/test
```

### Uninstallation

Remove existing configuration in `/etc/ssh/sshd_config`

Remove `shared` user

```
sudo groupdel shared
sudo deluser --remove-home shared
```

## OpenJDK 8

### Installation

```
sudo apt update
sudo apt install openjdk-8-jdk
sudo update-alternatives --config java
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

### Uninstallation

```
sudo apt purge openjdk-8*
```

## Nginx


### Installation

```
sudo wget https://nginx.org/keys/nginx_signing.key
sudo apt-key add nginx_signing.key
sudo nano /etc/apt/sources.list.d/nginx.list
deb [arch=amd64] http://nginx.org/packages/mainline/ubuntu/ bionic nginx
deb-src http://nginx.org/packages/mainline/ubuntu/ bionic nginx
sudo apt remove nginx nginx-common nginx-full nginx-core
sudo apt update
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl stop nginx
```

### Uninstallation

```
sudo apt purge nginx*
```

## Keepalived

### Installation

```
sudo apt install keepalived
sudo vim /etc/keepalived/keepalived.conf
sudo systemctl start keepalived
sudo systemctl restart keepalived
sudo systemctl status keepalived
sudo systemctl stop keepalived
```

### Uninstallation

```
sudo apt purge keepalived*
```

## Fix file limit warning on VSCode

```
echo "fs.inotify.max_user_watches=524288" >> /etc/sysctl.conf
sudo sysctl -p
```
