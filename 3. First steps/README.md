# First steps

## How to install docker

### Terminal

On Linux you can use the CLI based verison of Docker: https://docs.docker.com/engine/

### Docker Desktop

Follow the official installation: 
- Linux: https://docs.docker.com/desktop/setup/install/linux/ubuntu/
- Windows: https://docs.docker.com/desktop/setup/install/windows-install/
- Mac: https://docs.docker.com/desktop/setup/install/mac-install/

# Running your first container

```bash
$ docker run hello-world

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
When you run this command:
- The Docker client contacted the Docker daemon
- Docker daemon checks if the `hello-world` image is available locally.
- If not, it automatically downloads (aka "pulls") the image from Docker Hub.
- Docker daemon creates a new container based on this image.
- The container runs, the container's output was sent back to your terminal, and then the container exits.

## Docker image layers
- Docker images are built using a layered filesystem
- Each layer represents a set of filesystem changes, applied to the previous layer
- Each layer corresponds to a command in the `Dockerfile`
- To see the layers of an image:
    - each sha256 hash is a layer
```
$ docker inspect --format='{{.RootFS.Layers}}' <image-name>
[sha256:6a7f953ae30c9f480e6eaf7be8b1ba742bce57a3a83c43e927348e763cff7472 sha256:47eeda9418868d99763d3843150298589075da5317bfc98559882901d214e459 sha256:83eccd85c69cb031721e0594289575c7319168f0e0c822573729bbd572e69dc5 sha256:f2ddb60a28da2b86001d5c1a6418a661bf901bad06654ae71e3ae425977f1457 sha256:df20eaef6952795dc3899be991cdafc372ab4e4b51fadddb1f98d7a74f31cd0a sha256:b1484d87e77a0d9caade3eeeab4bfe50541c7b51e923b159a8f0cada206fa2b4 sha256:396214f7f8fc12fa1270ab1103263098b028017560b6be7bd0abb208c09536d9]
```
- Layers are cached to speed up builds of similar images
- Layers are shared between images to save disk space
- When pushing or pulling only the changed layers are transferred


# Saving and Loading Docker images
- You can save images as .tar files
    - For transferring between system or for backup
- To save an image as a .tar into your current directory 
```bash
docker save nginx > nginx.tar
```
- To load an image from a .tar file
```bash
docker load < nginx.tar
```

# Image Tagging
- Tagging is like creating aliases for your Docker images
- Commonly used for organizing and versioning
```bash
$ docker tag <source_image_tag> <new_image_name>:<tag>
```
- Note the two images with the same Image ID:
```bash
$ docker tag nginx:latest my-nginx:v1
$ docker images
my-nginx       v1        058f4935d1cb   2 days ago       152MB
nginx          latest    058f4935d1cb   2 days ago       152MB
```

# Modes of containers
- You can run a container in `detached` or `interactive` mode
- By default containers are in `interactive` mode
```bash
$ docker run nginx
```
- Use the `-d` option to run a container in `detached` mode
```bash
$ docker run -d nginx`
```