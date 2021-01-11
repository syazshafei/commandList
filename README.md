# PostgreSQL
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

# Anaconda Environment
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

# Superset Installation
## Initializing Database
To initialize the database with a user and example charts, dashboards and datasets run:
```
docker-compose run -e SUPERSET_LOAD_EXAMPLES=yes --rm superset ./docker-init.sh
```
## Normal Operation
To run the container, simply run:
```
docker-compose up
```
To re-build docker image
```
docker-compose up --build -d
```

# Kill port/service on machine
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

# Docker
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
docker rmi [IMAGE]
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

# Druid
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

# SCP Linux
Copy file from a remote host to local host SCP example
```
scp username@from_host:file.txt /local/directory/
```
Copy file from local host to a remote host SCP example:
```
scp file.txt username@to_host:/remote/directory/
```

# Check storage
Check local disk space:
```
df -h
```
List all & hidden file:
```
ls -la
```
Get size of all the directories
```
du -h --max-depth=1
```
Find file recusively
```
find -L . -name "historical*"
```
