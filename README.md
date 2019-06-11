####PostgreSQL
Connect to postgres user on postgreSQL
```
psql -h localhost -U postgres
```

####Anaconda Environment
Create environemnt
```
conda create -n yourenvname python=x.x anaconda
```
Delete environment
```bash
conda remove --name yourenvname --all
```

####Superset Installation

Frontend Assets

Install third-party dependencies listed in `package.json`:

```bash
# From the root of the repository
cd superset/assets

# Install dependencies from `package-lock.json`
npm ci
```

Build the Javascript bundles
```bash
npm run build
```


Alternatively you can use one of the following commands.

```bash
# Start a watcher that recompiles your assets as you modify them (but have to manually reload your browser to see changes.)
npm run dev

# Compile the Javascript and CSS in production/optimized mode for official releases
npm run prod

# Copy a conf file from the frontend to the backend
npm run sync-backend
```

Install external dependencies
```
cd incubator-superset
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

Install Superset in editable (development) mode
```bash
pip install -e .
```

Create an admin user
```bash
fabmanager create-admin --app superset
```

Initialize the database
```bash
superset db upgrade
```

Create default roles and permissions
```bash
superset init
```

Load some data to play with
```bash
superset load_examples
```

Start the Flask dev web server from inside the `superset` dir at port 8088
```bash
cd superset
FLASK_ENV=development flask run --host=0.0.0.0 -p 8088 --with-threads --reload --debugger
```

####Kill port on machine
```bash
sudo netstat -nltp|grep LISTEN
sudo netstat -nltp|grep :8080
sudo kill -TERM xxxxx
```

####Docker 
List docker container
```bash
docker ps
```
Run a command in a running container
```bash
docker exec
```
Copy files/folders between a container and the local filesystem
```bash
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Example:
docker cp /home/mimos/2015.csv e76ae9f4fca8:/druid/druid-0.12.3/data
```

####Druid
Ingest data into Druid
```bash
curl -X 'POST' -H 'Content-Type:application/json' -d @quickstart/tutorial/wikipedia-index.json http://localhost:8090/druid/indexer/v1/task
```

Query
```bash
curl -X 'POST' -H 'Content-Type:application/json' -d @query/detection-data-sql-minmaxdate.json http://localhost:8082/druid/v2/sql
```