# de_docker_ws

Docker commands:
1. docker run ubuntu
2. docker run -it ubuntu
3. apt update
4. apt install python3
the docker container has exactly those dependencies as described in the image and are stateless
it store the version of those containers 
5. docker ps -a
6. docker ps -aq -> gets the container id's
7. docker rm `docker ps -aq` -> removes the containers

to move data from dir. (of the test dir) to container 
8. docker run -it --entrypoint=bash -v $(pwd)/test:/app/test python:3.13.11-slim