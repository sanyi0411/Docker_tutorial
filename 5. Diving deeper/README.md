# Diving deeper

## See image layers
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

## Searching Docker Hub
- You can search for images on the Docker Hub webiste
- Or you can search directly from your terminal using a command line tool:
    - This tool does not support searching for specific tags
```bash
$ docker search nginx
NAME                  DESCRIPTION                                     STARS   OFFICIAL AUTOMATED
nginx                 Official build of Nginx.                        15763   [OK]
jwilder/nginx-proxy   Automated Nginx reverse proxy for docker c...   2088             [OK]
...
```

## Saving and Loading Docker images
- You can save images as .tar files
    - For transferring between systems or for backup
- To save an image as a .tar into your current directory: 
```bash
docker save nginx > nginx.tar
```
- To load an image from a .tar file:
```bash
docker load < nginx.tar
```

## Image Tagging
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

## Inspecting containers
- To get detailed information about a container in JSON format:
```bash
$ docker inspect nginx-detached
```
- This can be overwhelming
- Use the `-f` option to filter the output
```bash
$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nginx-detached
```
```bash
$ docker inspect -f '{{.State.Status}}' nginx-detached
```
- You can get certain details with different commands
```bash
$ docker port nginx-detached
```

## Port mapping
- In general port mapping (or port forwarding) is an application of Network Address Translation (NAT). It redirects communication coming to a specific port on the router's public IP to a designated device and port on the private internal network.
![port-forwarding](images/port-forwarding.svg)
- In Docker services inside a container are not reachable from the host by default
- Port mapping allows containers to communicate with the host system or external networks. It maps a port on the host system to a port inside the container. It creates a bridge between a host port and a container port
![docker-port-mapping](images/docker_port_forwarding.png)
- By default there are no ports mapped on a container
- To create a container with port mapping:
```bash
$ docker run -p <host_port>:<container_port> <image>
```
- Try to create an `nginx` container with port mapping
    - Use the `-d` option to run the container in the background
```bash
$ docker run -d -p 8080:80 nginx
```
- What happens now:
    - Docker allocates a port on the host
    - Adds iptables (NAT) rules on the host
    - Forwards incoming traffic: traffic to host port -> container IP and port
    - Handles return traffic back to the client
- If you now try to connect to `http://localhost:8080`, it will be forwarded to the containers port 80
- You can map multiple ports at the same time:
```bash
$ docker -d -p 8080:80 -p 8443:443 nginx
```
- You can let Docker choose a random port on the host:
```bash
$ docker run -p 80 nginx
1a2b3c4d
$ docker port 1a2b3c4d
80/tcp -> 0.0.0.0:49153
```
- You can bind a specific host interface to a container port
    - Following example only allows connection from the localhost
    - Good for security
```bash
$ docker run -d -p 127.0.0.1:8080:80
```