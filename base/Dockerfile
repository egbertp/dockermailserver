FROM ubuntu:14.04

MAINTAINER Egbert Pot <egbert@adcombi.com>

ENV DEBIAN_FRONTEND noninteractive

# Fix apt-get errors
RUN locale-gen en_US.UTF-8

# Update apt and upgrade to latest packages
RUN apt-get update && \
    apt-get -y upgrade

# default config
ENV APP_HOST mail.weboak.nl
ENV DB_HOST localhost
ENV DB_NAME virtual_mail
ENV DB_USER virtual_mail

# Configure postfix installation
RUN echo "postfix postfix/main_mailer_type string Internet site" | debconf-set-selections
RUN echo "postfix postfix/mailname string $APP_HOST" | debconf-set-selections

# Configure dovecot installation
# Dovecot wants to create an SSL certificate if there is no one available
# RUN echo "dovecot-core dovecot-core/create-ssl-cert boolean true | debconf-set-selections

# Install Postfix and Dovecot
RUN apt-get update && apt-get install -y \
    postfix \
    postfix-mysql \
    opendkim \
    mailutils \
    opendkim-tools \
    sasl2-bin \
    dovecot-common \
    dovecot-pop3d \
    dovecot-imapd \
    dovecot-mysql \
    mysql-client-core-5.5

# Clean apt
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ADD etc/postfix /etc/postfix
ADD etc/dovecot /etc/dovecot

RUN mkdir -p /srv/vmail

RUN groupadd -g 5000 vmail && \
    useradd -g vmail -u 5000 vmail -d /srv/vmail -m && \
    chgrp postfix /etc/postfix/mysql-*.cf && \
    chgrp vmail /etc/dovecot/dovecot.conf && \
    chmod g+r /etc/dovecot/dovecot.conf

RUN postconf -e virtual_uid_maps=static:5000 && \
    postconf -e virtual_gid_maps=static:5000 && \
    postconf -e virtual_mailbox_domains=mysql:/etc/postfix/mysql-virtual-mailbox-domains.cf && \
    postconf -e virtual_mailbox_maps=mysql:/etc/postfix/mysql-virtual-mailbox-maps.cf && \
    postconf -e virtual_alias_maps=mysql:/etc/postfix/mysql-virtual-alias-maps.cf,mysql:/etc/postfix/mysql-email2email.cf && \
    postconf -e virtual_transport=dovecot && \
    postconf -e dovecot_destination_recipient_limit=1 && \
    # specially for docker
    postconf -F '*/*/chroot = n'

RUN echo "# Added via Dockerfile" >> /etc/postfix/master.cf && \
    echo "dovecot   unix  -       n       n       -       -       pipe"  >> /etc/postfix/master.cf && \
    echo '    flags=DRhu user=vmail:vmail argv=/usr/lib/dovecot/deliver -d ${recipient}' >> /etc/postfix/master.cf


# SMTP ports
EXPOSE 25
EXPOSE 587  
# POP and IMAP ports  
EXPOSE 110
EXPOSE 143
EXPOSE 995
EXPOSE 993

# Add local files and make bin files executable
ADD /app/. /app
RUN chmod +x /app/bin/*

# Set the database hostname, username and password in the config files
RUN /app/update_config_templates.sh

# Rename executables
# RUN mv /app/bin/confd* /app/bin/confd

WORKDIR /app
VOLUME ["/etc/ssl"]
VOLUME ["/srv/vmail"]

CMD ["/app/bin/forego", "start", "-r"]
