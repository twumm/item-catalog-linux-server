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

1. Create and start a Ubuntu linux server on Amazon Lightsail.

2. Add a user
    * `sudo adduser grader`

3. Give user root access
    * Make an empty file titled at `/etc/sudoers.d/grader`
    * Paste the following to grant root permission to grader `grader ALL=(ALL) NOPASSWD:ALL`

4. Update all packages by running this command:
`sudo apt-get upgrade`
`sudo apt-get update`

5. Create ssh keys for grader
`mkdir /home/grader/.ssh
chown grader:grader /home/grader/.ssh
chmod 700 /home/grader/.ssh
cp /root/.ssh/authorized_keys /home/grader/.ssh/
chown grader:grader /home/grader/.ssh/authorized_keys
chmod 644 /home/grader/.ssh/authorized_keys`

6. Configure the Uncomplicated Firewall (UFW)
Allow the following ports for the project - SSH (port 2200), HTTP (port 80), and NTP (port 123).
    * `sudo ufw allow 2200/tcp`
    * `sudo ufw allow 80/tcp`
    * `sudo ufw allow 123/tcp`
    * `sudo ufw enable`

7. Install Apache and mod_wsgi
    * Apache - `sudo apt-get install apache2`
    * mod_wsgi - `sudo apt-get install libapache2-mod-wsgi python-dev`
    * Activate mod_wsgi - `sudo a2enmod wsgi`

8. Install git - to be used to clone the repository with the application - `sudo apt-get install git`

9. Clone the Item Catalog App from Github
    





# Software installed

* 
