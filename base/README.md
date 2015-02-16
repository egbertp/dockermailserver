A mail server in a box.
==========

Issuing either of the commands below will create am IMAP server with any important data safely keep in a persistent container. 

WORK IN PROGRESS!

Create MySQL database on an external database server

```bash
    $ mysql -u root -p 
```

```mysql
    mysql> CREATE DATABASE virtual_mail CHARACTER SET utf8 COLLATE utf8_general_ci;
    mysql> GRANT ALL PRIVILEGES ON virtual_mail.* TO 'virtual_mail'@'%' IDENTIFIED BY 'your-secret-password';
    mysql> FLUSH PRIVILEGES;
```
