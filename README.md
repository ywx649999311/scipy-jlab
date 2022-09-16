# scipy-jlab
A custom Docker image inspired by/built on top of [Jupyter Docker Stacks](https://github.com/jupyter/docker-stacks). For added Jupyet Lab extensions, see [extensions](./extension-list.md). 

This image works in the same way as you would use the official Jupyter Docker Stacks, but with addtional Jupyter Lab extensions and other custom terminal configurations. The docker image is automatically built. To use it, pull the image from Dockerhub with:
```
docker pull wyu16/scipy-jlab
```

## Run Server
```bash
docker run -d -v HOMEDIR:DOCKERDIR -p HOSTPORT:8888 --name container_name imageID start.sh jupyter lab --NotebookApp.passwork="sha:Password"
```