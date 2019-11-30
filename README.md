# Linux Server Configuration

## Project Overview

A baseline installation of a Linux server was prepared to host the Item Catalog web application. 
The server was secured from a number of attack vectors, a database server installed and configured, and the Item Catalog 
 web application deployed onto it.

## Server IP
IP: 13.54.205.130

SSH Port: 2200/tcp

## Web Application URL
URL: http://catalog.sweetfancycoding.live/

**Note**: configure the 'hosts' file on the client machine with the IP/hostname

## Software installed:

Apache Web Server

    sudo apt install apache2

Mod-wsgi (python 3)

    sudo apt-get install libapache2-mod-wsgi-py3

PostgreSQL database

    sudo apt install make zip unzip postgresql

Git version control system

    sudo apt install git

Python development tools

    sudo apt-get install build-essential libssl-dev libffi-dev python3-dev

Python 3 package manager (Pip)

    sudo apt install python3-pip


Python virtual environment manager (virtualenv)

    pip3 install virtualenv



## Configuration:

### Using default Lightsail ubuntu account (superuser permissions):

- Configure SSH Server to listen on port 2200/tcp
- Configure Amazon Lightsail Firewall to allow ports 80/tcp (www) and 2200/tcp (custom port)
- Configure Ubuntu UFW to allow all traffic outbound (outgoing)
- Configure Ubuntu UFW to deny all traffic inbound (incoming)
- Configure Ubuntu UFW to allow incoming 2200/tcp
- Configure Ubuntu UFW to allow incoming 80/tcp
- Configure Ubuntu UFW to allow incoming 123/tcp (ntp)
- Enable UFW

#### User with superuser permissions:
- Create user 'grader'
- Add 'grader' to group 'sudo' for superuser permissions

#### SSH access:

- Create directory '.ssh' under 'grader' home directory '/home/grader'
- Create file '/home/grader/.ssh/authorized_keys'
- Add public key created with 'ssh-keygen' to '/home/grader/.ssh/authorized_keys'
- Set permissions on '.ssh' and 'authorized_keys' to prohibit access by other users

        sudo chmod 644 .ssh/authorized_keys
        sudo chmod 755 .ssh
- Change owner of '.ssh' (recursively) to 'grader'

        sudo chown -R grader:grader grader/.ssh/
- Disable root login via SSH

        'PermitRootLogin no'


### Using 'grader' account (superuser permissions):

- Create 'catalog' PostgreSQL database

        sudo -u postgres createdb catalog
- Create PostgreSQL user 'catalog' with a password

        sudo -u postgres createuser -DRSP catalog

- Change authentication to md5 for local connections to allow login using psql
- Clone the 'item-catalog-web-application' GitHub repository
- Deploy the web application per the readme file

**Exceptions:**

1. Database configuration related environment variables
2. Python virtual environment manager - use 'virtualenv' rather than 'venv' so that required virtual environment activation files are available

- Create a 'wsgi' file to allow Apache to start the web application per the Flask documentation
- Add database related configuration to the python environment via the 'wsgi' file
- Change database username in 'app.py' to match 'catalog' user
- Change 'client_secret.json' path references to absolute



## External References:

Apache configuration

https://cwiki.apache.org/confluence/display/HTTPD/DistrosDefaultLayout#DistrosDefaultLayout-Debian,Ubuntu(Apachehttpd2.x)

Add user to group

https://www.cyberciti.biz/faq/howto-linux-add-user-to-group/


Convert rsa private key to putty private key

https://support.rackspace.com/how-to/log-into-a-linux-server-with-an-ssh-private-key-on-windows/

Create PostgreSQL user

https://www.postgresql.org/docs/9.1/app-createuser.html

Delete PostgreSQL user

https://www.postgresql.org/docs/9.1/app-dropuser.html

Change authentication to md5 for local connections to allow login using psql

https://www.postgresql.org/docs/9.1/auth-methods.html

Install python development tools

https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-16-04-server


Deploy Flask app on Apache with 'mod_wsgi'

https://flask.palletsprojects.com/en/1.1.x/deploying/mod_wsgi/

Add environment variables via wsgi file

https://gist.github.com/GrahamDumpleton/b380652b768e81a7f60c









