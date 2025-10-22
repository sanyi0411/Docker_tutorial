# Docker notes
This repository contains the notes I took during learning about Docker and containerization

# Glossary

**Container**: A lightweight, standalone, and executable package that includes everything needed to run a piece of software.

**Image**: Template or blueprint for containers. It contains all the instructions needed to create a container. Images are used to create containers.

**Dockerfile**: The script used to build the image.

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Docker Engine**: The core technology that runs and manages containers on your machine. It can pull images from Docker Hub and push images to Docker Hub

# How to install docker

# Running your first container

```
> docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:...
Status Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```

**Docker client**: the part you interact with

**Docker daemon**: the background service that manages Docker on your machine

When you run this command:
- The Docker client contacted the Docker daemon
- Docker daemon checks if the hello-world image is available locally.
- If not, it automatically downloads (aka "pulls") the image from Docker Hub.
- Docker daemon creates a new container based on this image.
- The container runs, the container's output was sent back to your terminal, and then the container exits.

# Docker images

**Docker image**: an image is like a blueprint or a template for a container

To see the images available on your local system:
```
> docker images

REPOSITORY          TAG       IMAGE ID       CREATED         SIZE
hello-world         latest    feb5d9fea6a5   2 weeks ago     13.3kB
```

The `hello-world` image is now stored locally on your system. This means that if you run `docker run hello-world` again, Docker won't need to download the image from Docker Hub. It will use the local copy, making the process faster.

**REPOSITORY**: the name of the image

**TAG**: The version of the image. "latest" is the default tag

**IMAGE ID**: A unique identifier for the image (useful when you need to refer to a specific image)

**CREATED**: The time when the image was created (helps you know if you have the most recent version)

**SIZE:**: The size of the image on disk

# Docker hub

https://hub.docker.com/

**Docker Hub**: A cloud-based registry, serving as a central repository, service where you can store, find and share container images.

**Official Images**: Curated by Docker and are typically well-maintained and documented.

**Pull Command**: Command to manually download the image without running a container.
```
docker pull hello-world
```

# Docker containers
```
> docker ps

CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS          PORTS     NAMES
f3f3b3b3b3b3   docker/getting-started   "/docker-entrypoint.…"   1 minute ago   Up 1 minute   80/tcp   festive_mendel
```
