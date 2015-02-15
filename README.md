A mail server in a box.
==========

[2015-02-15 16:57:13 CEST] WARNING: THIS PROJECT IS STILL WORK IN PROGRESS

This project is heavily inspired by https://github.com/lihop/dockermail and https://github.com/htmlgraphic/Mail-Server/


A secure, minimal-configuration mail server in a docker container, including webmail.

This repository is tailored to small private servers, where you own some domain(s) and
want to receive the mail for and send mail from this domain. It consists of  separate docker containers:

 - **dovecot**:  The SMTP and IMAP server. This container uses postfix as MTA and dovecot as IMAP server.
    All incoming mail to your own domains is accepted. For outgoing mail, only authenticated (logged in with username and password)
    clients can send messages via STARTTLS on port 587. In theory it works with all mail clients, but it was only tested with Thunderbird.

 - **rainloop**: An automatically configured webmail interface. Note that you have to login with your full mail adress, 
   e.g. `john.doe@example.org` instead of just `john.doe`. By default, this will bind to `localhost:33100`.

   Rainloop is released under CC BY-NC-SA 3.0, so you are only allowed to use this container for non-commercial purposes. They do
   sell commercial licenses, if required.


Setup
=====


###1) Add needed domains 


###2) Add user aliases 

###3) Add user passwords

To get the hash values, you can either install dovecot locally or use lxc-attach to attach to the running
container and run `doveadm pw -s <scheme-name>` inside.

>###You can stop here if you only would like a IMAP server setup

---


Known issues / Todo / Wishlist
==============================
