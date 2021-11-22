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
