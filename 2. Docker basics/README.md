# Docker basics

## Table of contents
[Documentation](#documentation) </br>
[History](#history) </br>
[Glossary](#glossary) </br>
[Basic Docker architecture](#basic-docker-architecture) </br>
[Docker images](#docker-images) </br>
[Docker hub](#docker-hub) </br>
[Docker containers](#docker-containers) </br>

## Documentation

Official Docker documentation: https://docs.docker.com/

## History

- Inventor: Solomon Hykes
- 2013 March: First public announcement at PyCon
- 2013 September: RedHat collaboration
- 2014: Microsoft announces Windows integration

- All Docker versions are backwards compatible

## Glossary

**Dockerfile**: The script used to build the image. Docker builds images by reading the instructions from a Dockerfile. It contains instructions for building your source code. See specification [here](https://docs.docker.com/reference/dockerfile/)

**Image**: Template or blueprint for containers. It contains all the instructions needed to create a container. Images are used to create containers.

**Container**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software.

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Docker Engine**: The core technology that runs and manages containers on your machine. It can pull images from Docker Hub and push images to Docker Hub

**Docker client** (`docker`): the part you interact with. The client sends your commands to `dockerd`. The client can communicate with more than one daemon.

**Daemon**: an application in Linux that runs in the background. If configured it will start with the OS if the computer is restarted

**Docker daemon** (`dockerd`): the background service that manages Docker objects (e.g.: images, containers, networks, volumes) on your machine

## Basic Docker architecture

```
+-----------+        +-----------------------------------+          +--------------+
|  CLIENT   |        |                HOST               |          |   REGISTRY   |   
|           |        |                                   |          |              |
|           |        |    +-------------------------+    |          |  Docker Hub  |
|  Terminal | ---->  |    |      Docker Daemon      |    | ------>  |              |
|           | <----  |    |                         |    |          |              |
|    or     |        |    +-------------------------+    |          |              |
|           |        |                                   |          |    Images    |
|  Docker   |        |                                   |          |  Extensions  |
|  Desktop  |        | +------------+       +----------+ |          |    Plugins   |
|           |        | | Containers | <---- |  Images  | |  <-----  |              |
|           |        | +------------+       +----------+ |          |              |
|           |        |                                   |          |              |
+-----------+        +-----------------------------------+          +--------------+
 
```

Docker is using the Linux kernel to run the containers

On Windows and Mac there is a Linux virtual machine created for you that runs the containers

## Docker images

- **Docker image**: an image is like a blueprint or a template for a container

    - To see the images available on your local system:
        - `docker images` is the same as `docker image ls`
    ```bash
    $ docker images
    REPOSITORY     TAG     IMAGE ID       CREATED       SIZE
    hello-world    latest  feb5d9fea6a5   2 weeks ago   13.3kB
    ```

- The `hello-world` image is stored locally on your system. This means that if you run `docker run hello-world` again, Docker won't need to download the image from Docker Hub. It will use the local copy, making the process faster.

- **REPOSITORY**: the name of the image
- **TAG**: The version of the image. "latest" is the default tag
    - Docker allows you to run specific versions of an image by using tags.
    - For example you can download multiple python images:
    - To see all python images on your local system:
    ```bash
    $ docker images python
    REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
    python       latest    1edf53fea028   2 days ago    1.12GB
    python       3.7       16d93ae3411b   2 years ago   994MB
    ```
- **IMAGE ID**: A unique identifier for the image (useful when you need to refer to a specific image)
- **CREATED**: The time when the image was created (helps you know if you have the most recent version)
- **SIZE**: The size of the image on disk

- To remove an image from your system:
    - `rmi`: remove image
    ```bash
    $ docker rmi python:3.7
    ```
    - If you get the following error, then there is a container (running or stopped) create from the image you are trying to delete
    ```
    Error response from daemon: conflict: unable to remove repository reference "python:3.7" (must force) - container <container_id> is using its referenced image <image_id>
    ```
    - look for the image using `docker ps -a`
    - and delete the container using `docker rm <container_id>`

## Docker hub

https://hub.docker.com/

- **Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images. A public repository for Docker images. You can find pre-built images for many popular software applications. 
- **Official Images**: Curated by Docker and are typically well-maintained and documented.

- **Pull Command**: Command to manually download the image without running a container.
    - Q: Why use `docker pull` and not just `docker run`?
    - A: `docker runs` includes `docker pull` so you don't need to separately run `docker pull`. But if you know what the image you will be using, you can download it beforehand with `docker pull`. Later, when you want to start your container, it will start up much faster because the image is already downloaded and available locally.
    ```bash
    $ docker pull hello-world
    ```

### Searching Docker Hub
- You can search for images on the Docker Hub webiste
- Or you can search directly from you terminal using a command line tool:
    - This tool does not support searching for specific tags
```bash
$ docker search nginx
NAME                  DESCRIPTION                                     STARS   OFFICIAL AUTOMATED
nginx                 Official build of Nginx.                        15763   [OK]
jwilder/nginx-proxy   Automated Nginx reverse proxy for docker c...   2088             [OK]
...
```

## Docker containers
```bash
$ docker ps
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS     NAMES
f3f3b3b3b3b3   docker/getting-started   "/docker-entrypoint.…"   1 minute ago   Up 1 minute   80/tcp   festive_mendel
```

