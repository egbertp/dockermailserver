FROM ubuntu:14.04

MAINTAINER Egbert Pot <egbert@adcombi.com>

ENV DEBIAN_FRONTEND noninteractive

# Fix apt-get errors
RUN locale-gen en_US.UTF-8 && dpkg-reconfigure locales

# Make sure that the Ubuntu packages are loaded from the 
# correct Ubuntu repository: add sources.list
ADD etc/apt/sources.list /etc/apt/sources.list

# Update apt and upgrade to latest packages
RUN apt-get update && \
    apt-get -y upgrade

RUN apt-get install -y openssl

VOLUME ["/etc/ssl"]

# Add local files and make bin files executable
ADD /app/. /app
RUN chmod +x /app/bin/*

# Rename executables
RUN mv /app/bin/confd* /app/bin/confd

WORKDIR /app
CMD ["/app/bin/forego", "start", "-r"]

# Clean apt
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*