# Linux Server Configuration Project

This project demos an item catalog web application running on a Linux server(Amazon Lightsail).

# Getting Started

To view the application, visit [Item Catalog](http://18.194.44.53/).

* The public IP address for the server is `18.194.44.53`
* SSH port is `2200`

To login as grader, use this command:
`ssh grader@18.194.44.53 -p 2200 -i ~/linuxCourse`

Create the `linuxCourse` file by creating an empty file and pasting the content of the `grader` user's SSH key.

## Configuration Steps

### Create and start a Ubuntu linux server on Amazon Lightsail.

### Add a user
* `sudo adduser grader`

### Give user root access
a. Make an empty file titled at `/etc/sudoers.d/grader`
b. Paste the following to grant root permission to grader `grader ALL=(ALL) NOPASSWD:ALL`

### Update all packages by running this command:
* `sudo apt-get upgrade`
* `sudo apt-get update`

### Create ssh keys for grader
`mkdir /home/grader/.ssh`
`chown grader:grader /home/grader/.ssh`
`chmod 700 /home/grader/.ssh`
`cp /root/.ssh/authorized_keys /home/grader/.ssh/`
`chown grader:grader /home/grader/.ssh/authorized_keys`
`chmod 644 /home/grader/.ssh/authorized_keys`

### Configure the Uncomplicated Firewall (UFW)
Allow the following ports for the project - SSH (port 2200), HTTP (port 80), and NTP (port 123).
* `sudo ufw allow 2200/tcp`
* `sudo ufw allow 80/tcp`
* `sudo ufw allow 123/tcp`
* `sudo ufw enable`

Add port 2200 to the *sshd_config* file:
* Run `sudo nano /etc/ssh/sshd_config`
* Add **Port 2200** under **Port 22** and save the file

Once you test the connection and you can successfully connect to the server using port 2200, edit the **sshd_config** file again and delete the **Port 22**

### Install Apache and mod_wsgi
* Apache - `sudo apt-get install apache2`
* mod_wsgi - `sudo apt-get install libapache2-mod-wsgi python-dev`
* Activate mod_wsgi - `sudo a2enmod wsgi`

### Install and setup the database:
* Run `sudo apt-get install postgresql` to install PostgreSQL
* Login in a postgres user `sudo su - postgres`. Run the postgreSQL shell - `psql`
* Create new database for items and user
`postgres=# CREATE DATABASE catalog;`
`postgres=# CREATE USER catalog;`
* Set password for the db and quit postgresql
    

### Install Git and Clone the Item Catalog App from Github
* Install Git - `sudo apt-get install git`
* Make a new directory `sudo mkdir /var/www/FlaskApp`
* Open the directory `cd /var/www/FlaskApp`
* Clone the application you want to work on (Item Catalog App in this situation) `git clone [github repository link]`
* Change the name of the clone's repo to *FlaskApp* `sudo mv ./[folder name] ./FlaskApp`
* Open the repo folder - `cd FlaskApp`
* Rename the main application to `__init__.py` `sudo mv application.py __init__.py`
* Change the db call to `engine = create_engine('postgresql://catalog:password@localhost/catalog')`
* Install pip - `sudo apt-get install pytho-pip`. Run `sudo pip install -r requirements.txt` to install various dependencies for the application
* Install psycopg2 `sudo apt-get -qqy install postgresql python-psycopg2`
* Create database `sudo python database_setup.py`
* Populate db items `sudo python itemsdb.py`

### Enable Virtual Host
* Create `FlaskApp.conf`. Edit this file - `sudo nano /etc/apache2/sites-available/FlaskApp.conf`
* Add these details
```
<VirtualHost *:80>
ServerName 000.000.000.000
ServerAdmin me@email.com
WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
<Directory /var/www/FlaskApp/FlaskApp/>
	Order allow,deny
	Allow from all
</Directory>
Alias /static /var/www/FlaskApp/FlaskApp/static
<Directory /var/www/FlaskApp/FlaskApp/static/>
	Order allow,deny
	Allow from all
</Directory>
ErrorLog ${APACHE_LOG_DIR}/error.log
LogLevel warn
CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### Create and edit the wsgi File
* `sudo nano /var/www/FlaskApp/flaskapp.wsgi`
* Add this code to the flaskapp.wsgi file
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/FlaskApp/")

from FlaskApp import app as application
application.secret_key = 'Add your secret key'
```

### Restart Apache
To get the application running - `sudo service apache2 restart`

## References 
[Digital Ocean - How to deploy a flask app](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
