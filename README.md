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
Create environemnt
```
conda create -n yourenvname python=x.x anaconda
```
Delete environment
```
conda remove --name yourenvname --all
```

# Superset Installation

OS dependencies
```
sudo apt-get install build-essential libssl-dev libffi-dev python-dev python-pip libsasl2-dev libldap2-dev
```
For python3
```
sudo apt-get install build-essential libssl-dev libffi-dev python3.6-dev python-pip libsasl2-dev libldap2-dev
```

Frontend Assets

Install third-party dependencies listed in `package.json`:

```
## From the root of the repository
cd superset/assets

## Install dependencies from `package-lock.json`
npm ci
```

Build the Javascript bundles
```
npm run build
```


Alternatively you can use one of the following commands.

```
## Start a watcher that recompiles your assets as you modify them (but have to manually reload your browser to see changes.)
npm run dev

## Compile the Javascript and CSS in production/optimized mode for official releases
npm run prod

## Copy a conf file from the frontend to the backend
npm run sync-backend
```

Developers should use a virtualenv.
```
pip install virtualenv
```
Then proceed with:
```
<!-- # Create a virtual environemnt and activate it (recommended) -->
virtualenv -p python3 venv # setup a python3.6 virtualenv
source venv/bin/activate

<!-- # Install external dependencies -->
pip install -r requirements.txt
pip install -r requirements-dev.txt

<!-- # Install Superset in editable (development) mode -->
pip install -e .

<!-- # Create an admin user in your metadata database -->
fabmanager create-admin --app superset

<!-- # Initialize the database -->
superset db upgrade

<!-- # Create default roles and permissions -->
superset init

<!-- # Load some data to play with -->
superset load_examples

<!-- # Start the Flask dev web server from inside your virtualenv.
# Note that your page may not have css at this point.
# See instructions below how to build the front-end assets. -->
FLASK_ENV=development superset run -p 8088 --with-threads --reload --debugger
```
If you have made changes to the FAB-managed templates, which are not built the same way as the newer, React-powered front-end assets, you need to start the app without the `--with-threads` argument like so: `FLASK_ENV=development superset run -p 8088 --reload --debugger`

# Superset Docker
## Initializing Database
To initialize the database with a user and example charts, dashboards and datasets run:
```
SUPERSET_LOAD_EXAMPLES=yes docker-compose run --rm superset ./docker-init.sh
```
or

```
docker-compose run --rm -e SUPERSET_LOAD_EXAMPLES=yes superset ./docker-init.sh
```
## Normal Operation
To run the container, simply run:
```
docker-compose up -d
```

# Kill port on machine
```
sudo netstat -nltp|grep LISTEN
sudo netstat -nltp|grep :8080
sudo kill -TERM xxxxx
```

# Docker
List docker container
```
docker ps
```
Run a command in a running container
```
docker exec
```
Copy files/folders between a container and the local filesystem
```
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Example:
docker cp /home/mimos/2015.csv e76ae9f4fca8:/druid/druid-0.12.3/data
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
docker save [OPTIONS] IMAGE [IMAGE...]
```
Load an image from a tar archive or STDIN
```
docker load [OPTIONS]
```

# Druid
Ingest data into Druid
```
curl -X 'POST' -H 'Content-Type:application/json' -d @quickstart/tutorial/wikipedia-index.json http://localhost:8090/druid/indexer/v1/task
```

Query
```
curl -X 'POST' -H 'Content-Type:application/json' -d @query/detection-data-sql-minmaxdate.json http://localhost:8082/druid/v2/sql
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
