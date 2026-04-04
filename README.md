# de_docker_ws

Docker commands:
1. docker run ubuntu (makes a container with the dep. init)
2. docker run -it ubuntu
3. apt update
4. apt install python3

the docker container has exactly those dependencies as described in the image and are stateless

it stores the version of those containers 

5. docker ps -a
6. docker ps -aq -> gets the container id's
7. docker rm `docker ps -aq` -> removes the containers

to move data from directory (of the test dir) to container

8. docker run -it --entrypoint=bash -v $(pwd)/test:/app/test python:3.13.11-slim

will move data from pwd/test to app/test

Making venvs's to seprate dependencies from the host machine (inside pipeline folder)
1. pip install uv
2. uv init --python 3.13
3. uv run python -V -> creates a .venv
4. which python & uv run which python
5. use that venv when running that .py file (change python interpreter, first install python extension)
6. uv run python pipeline.py ( source /workspaces/de_docker_ws/pipeline/.venv/bin/activate )

Making Dockerfile to make a container of pipeline with all py dependencies and run the script when that container runs

In pipeline folder:

1. build the Dockerfile: `docker build -t test:pandas .`

2. running the container: `docker run -it --entrypoint=bash --rm test:pandas`

We end up in /code dir which has pipeline.py as mentioned in Dockerfile

Or set up `ENTRYPOINT` in Dockerfile and just run: `docker run -it --rm test:pandas`

`--entrypoint=bash` sets up the place or entrypoint of us in the container when it runs

Running POSTGRES on Docker:
```bash
docker run -it --rm \
    -e POSTGRES_USER="root" \
    -e POSTGRES_PASSWORD="root" \
    -e POSTGRES_DB="ny_taxi" \
    -v ny_taxi_postgres_data:/var/lib/postgresql \
    -p 5432:5432 \
    postgres:18
```

`-v ny_taxi_postgres_data:/var/lib/postgresql` -> this makes internal volume in the container unlike where we used $(pwd) earlier and can store data

to interact with pg container use: `uv add --dev pgcli` (in pipeline dir) (--dev to not sync this in Dockerfile)

to connect to Postgres: `uv run pgcli -h localhost -p 5432 -u root -d ny_taxi`