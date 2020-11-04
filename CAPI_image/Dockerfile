# Specifying the ubuntu base image
FROM ubuntu
    
# Name and email of the person who maintains the file
MAINTAINER Marc TITINGER marc.titinger@hotmail.fr 
  
RUN useradd -m -s $(which bash) -G sudo marc
WORKDIR /home/marc

ENV DEBIAN_FRONTEND=noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN=true

RUN echo "tzdata tzdata/Areas select Europe" > tzdata.txt
RUN echo "tzdata tzdata/Zones/Europe select Berlin" >> tzdata.txt
RUN debconf-set-selections tzdata.txt
 
# Updating ubuntu and installing other necessary software
RUN apt-get update --yes \
&& apt-get install wget build-essential zlib1g-dev libncurses5-dev vim --yes
RUN apt-get install git cmake --yes

# Enable source URIs
RUN cp /etc/apt/sources.list /etc/apt/sources.list~
RUN sed -Ei 's/^# deb-src /deb-src /' /etc/apt/sources.list
RUN apt-get update
RUN apt-get build-dep dbus --yes

USER marc
RUN apt-get source dbus

# CAPI Runtime  
RUN git clone https://github.com/GENIVI/capicxx-core-runtime.git
RUN cd capicxx-core-runtime \
&& mkdir build && cd build && cmake .. && make

# CAPI DBUS
RUN git clone https://github.com/GENIVI/capicxx-dbus-runtime.git

# Patch DBUS (uninstalled version)
RUN cd dbus-1.12.16 \
&& for i in ../capicxx-dbus-runtime/src/dbus-patches/*.patch; do patch -N --verbose  -p1 < $i; done

RUN cd dbus-1.12.16 && ./configure && make -j 4

# so configure for CAPI-DBUS finfs the .pc for dbus. 
ENV PKG_CONFIG_PATH="/home/marc/dbus-1.12.16"

RUN cd capicxx-dbus-runtime && mkdir build && cd build \
&& cmake -DUSE_INSTALLED_COMMONAPI=OFF -DUSE_INSTALLED_DBUS=OFF .. \
&& make -j4 

# Set command to bash
CMD ["/bin/bash"]
