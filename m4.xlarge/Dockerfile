FROM ubuntu

#######################################################################
# Basic Setup
#

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
RUN apt-get update && apt-get -y upgrade

#######################################################################
# Basic toolkits
#

# Java
RUN apt-get -y install openjdk-8-jdk wget
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

# Misc
RUN apt-get -y install vim

#######################################################################
# Mount points for data directories
#

RUN mkdir -p /mnt/test_data
RUN mkdir -p /mnt/training_data
RUN mkdir -p /mnt/output
RUN mkdir -p /mnt/logs
RUN mkdir -p /mnt/temp

########################################################################

RUN echo "umask 0000" >> /root/.bashrc

WORKDIR /root
RUN ["/bin/bash"]
