# Specifying the ubuntu base image
FROM ubuntu
    
# Name and email of the person who maintains the file
MAINTAINER Marc TITINGER marc.titinger@hotmail.fr 
  
RUN useradd -m -s $(which bash) -G sudo marc
WORKDIR home/marc/WORK/fun-with-docker/arm-qemu


ENV DEBIAN_FRONTEND=noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN=true

#RUN echo "tzdata tzdata/Areas select Europe" > tzdata.txt
#RUN echo "tzdata tzdata/Zones/Europe select Berlin" >> tzdata.txt
#RUN debconf-set-selections tzdata.txt
 
# Updating ubuntu and installing other necessary software
RUN apt-get update --yes
RUN apt-get install locales --yes

RUN locale-gen "en_US.UTF-8"
RUN dpkg-reconfigure locales

RUN apt-get install  apt-utils wget build-essential zlib1g-dev libncurses5-dev vim --yes
RUN apt-get install python git cmake --yes
RUN apt install libcanberra-gtk-module libcanberra-gtk3-module --yes

# to build sabrelite yocto
RUN apt-get install gawk diffstat unzip texinfo chrpath libsdl1.2-dev xterm curl --yes

RUN apt-get install qemu-system-arm --yes
RUN apt-get install libc6-armel-cross libc6-dev-armel-cross binutils-arm-linux-gnueabi libncurses5-dev build-essential bison flex libssl-dev bc --yes
RUN apt-get install -y cpio
# Enable source URIs
#RUN cp /etc/apt/sources.list /etc/apt/sources.list~
#RUN sed -Ei 's/^# deb-src /deb-src /' /etc/apt/sources.list
#RUN apt-get update
#RUN apt-get build-dep dbus --yes


USER marc

ARG SOMEARG=blah

RUN echo "#!/bin/sh" > ~/go.sh
RUN echo "qemu-system-arm -machine sabrelite" >> ~/go.sh
RUN chmod +x ~/go.sh

##
RUN mkdir ~/bin
RUN curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
RUN chmod a+x ~/bin/repo 
ENV PATH="~/bin:${PATH}"
RUN mkdir ~/fsl-community-bsp
RUN cd ~/fsl-community-bsp ; ~/bin/repo init -u https://github.com/Freescale/fsl-community-bsp-platform -b dunfell
RUN cd ~/fsl-community-bsp ; ~/bin/repo sync

#RUN source ./setup-environment build
#RUN bitbake core-image-minimal

# Set command to bash
CMD ["/bin/bash"]
