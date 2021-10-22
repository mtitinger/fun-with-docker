Various Dockerfiles
===================

* CommonAPI 3 build and test
*

## Docker cheat sheet

### setting up docker

* sudo groupadd docker
* sudo usermod -aG docker $USER
* sudo chown $USER:docker /var/run/docker.sock 

For X windows routing: 

* HOST : sudo apt install libcanberra-gtk-module libcanberra-gtk3-module
* HOST : sudo apt install x11-xserver-utils
* HOST : xhost +
* docker run -it  --name qemucontainer --net host --privileged -e "DISPLAY=:1" -v /tmp/.X11-unix:/tmp/.X11-unix f3a9c9616291 /bin/bash

### building an image from a docker file
* docker build .
* docker images

### running a container
* docker ps -a

Running interactive (with shell)
* docker run -it  --name qemucontainer f3a9c9616291 /bin/bash

DEtached mode, publishing ports, and naming :
* docker run -d -P --name myshortname [repo/image]

Stopping a detached container
* docker stop myshortname

See ports from a container
* docker port myshortname

### removing images
* docker container prune
* docker rmi (image)

## Working with ports



