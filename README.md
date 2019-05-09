# Project: Linux Server Configuration

This project will take a baseline installation of a Linux server and prepare it to host your web applications. You will secure your server from a number of attack vectors, install and configure a database server, and deploy one of your existing web applications onto it.

## Server Information

* IP address: 18.237.100.85
* SSH port: 2200
* URL used to run the Catalog Item Project: [18.237.100.85.xip.io](http://18.237.100.85.xip.io)

## Software Packages Installed

| Package             | Description                                         |
|---------------------|-----------------------------------------------------|
| apache2             | Web Server                                          |
| libapache2-mod-wsgi | Allows the Apache web server to run Python web apps |
| PostgreSQL          | Relational Database                                 |
| Git                 | Distributed Version Control System                  |
| Python 2.7          | Programming Language                                |
| Flask               | Microframework for Python                           |
|                     |                                                     |

## Server Configuration Changes instructions

Sections: \
1  [Ubuntu Linux Server instance on Amazon Lightsail](
    #1.-ubuntu-linux-server-instance-on-amazon-lightsail) \
&nbsp;&nbsp; a. [Create the Amazon Web Server (AWS) instance](
    #a.-Create-the-Amazon-Web-Server-(AWS)-instance)\
&nbsp;&nbsp; b. [Log into the server using SSH](
    #b.-Log-into-the-server-using-SSH) \
2  [Secure your server](#secure-your-server) \
&nbsp;&nbsp; a. [Update-all-currently-installed-packages](
    #Update-all-currently-installed-packages) \
&nbsp;&nbsp; b. [Change the SSH port from 22 to 2200](
    #Change-the-SSH-port-from-22-to-2200) \
&nbsp;&nbsp; c. [Configure the Uncomplicated Firewall (UFW)](
    #Configure-the-Uncomplicated-Firewall-(UFW)) \
3  [Give grader access](#give-grader-access) \
&nbsp;&nbsp; a. [Create a new user account named grader](
    #Create-a-new-user-account-named-grader) \
&nbsp;&nbsp; b. [Create an SSH key pair for grader using the ssh-keygen tool](
    #Create-an-SSH-key-pair-for-grader-using-the-ssh-keygen-tool) \
&nbsp;&nbsp; c. [Create a new user account named grader](
    #Create-an-SSH-key-pair-for-grader-using-the-ssh-keygen-tool) \
4  [Prepare to deploy your project](#prepare-to-deploy-your-project) \
5  [Deploy the Item Catalog project](#deploy-the-item-catalog-project) \

## 1. Ubuntu Linux Server instance on Amazon Lightsail

### *a. Create the Amazon Web Server (AWS) instance*

1. Go to the <https://developer.amazon.com/>
2. Create an account and follow the prompts
3. Login to your account
4. Create an instance
    1. Pick your instance image: Select `Linux/Unix`
    2. Select a blueprint: Select `OS Only` \
        NOTE: The default is `App+OS` which is not recommended
    3. Select `Ubuntu`
5. Scroll down to the bottom
6. Click on the `Create instance` button

### *b. Log into the server using SSH*

1. Go to <https://aws.amazon.com/>
2. Login to your account
3. Go to <https://lightsail.aws.amazon.com/ls/webapp/home/instances>
4. Click on your instance. This will show more detailed information on your instance
5. Click on the `Connect using SSH` button

## Secure your server

### *Update all currently installed packages*

1. On the server issue the commands:

    > sudo apt-get update \
    > sudo apt-get upgrade

### *Change the SSH port from 22 to 2200*

1. On the lightsail management console, add the rule:

    | Field       | Value  |
    |-------------|------- |
    | Application | Custom |
    | Protocol    | TCP    |
    | Port        | 2200   |

2. Update the sshd_config file
    > sudo nano /etc/ssh/sshd_config
3. Search for `Port 22`
4. Replace it with `Port 2200`
5. Save the file
6. Restart the server to enable this change
    > sudo service restart sshd

### *Configure the Uncomplicated Firewall (UFW)*

1. Check firewall status
    > sudo ufw status
2. Close all incoming ports
    > sudo ufw default deny incoming
3. Open all outgoing ports
    > sudo ufw default allow outgoing
4. Open ssh port
    > sudo ufw allow 2200/tcp
5. Open http port
    > sudo ufw allow 80/tcp
6. Open ntp port
    > sudo ufw allow 123/udp
7. Turn on firewall
    > sudo ufw enable
8. Validate that the firewall is active with the changes
    > sudo ufw status

## Give grader access

### *Create a new user account named grader*

> sudo adduser grader

### *Give grader the permission to sudo*

   1. Go to `/etc/sudoers.d/` directory
   2. Edit the file
        > sudo nano 90-cloud-ini-users
   3. Search for the heading `# User rules for xxxx`, where xxx is the
        default user on server
   4. Add this line of code on its on line
        > grader ALL=(ALL) NOPASSWD:ALL
   5. Save the file

### *Create an SSH key pair for grader using the ssh-keygen tool*

   1. On your local machine, create an SSH key pair for grader using
        the ssh-keygen tool
   2. Deploy public key to your server instance
        > sudo mkdir .ssh
        > sudo touch .ssh/authorized_keys
        > sudo nano .ssh/authorized_keys
   3. Copy the contents of the created public key and paste into the
        authorized_keys file on the server
        machine and save
   4. Change the permissions to the `~/.ssh` directory
        > sudo chmod 700 .ssh
   5. Change the permissions to the `authorized_keys` file
        > sudo chmod 644 .ssh/authorized_keys
   6. Restart the server to use the changes
        > sudo service ssh restart
   7. Test the create user, log into the server using grader
        > ssh -i ~/.ssh/grader_key -p 2200 grader@18.237.100.85
   8. When prompted, enter grader's password
   9. Once logged in, update the configuration file to disable password access
        > sudo nano /etc/ssh/sshd_config
   10. Change PasswordAuthentication `yes` to `no`
   11. Restart the server to use the changes
        > sudo service ssh restart

## Prepare to deploy your project

### *Configure the local timezone to UTC*

   1. Check if the server is setup up the the UTC timezone
        > date
   2. If the date does not show up as UTC, use this command to update the
        date to UTC.  The Configuring tzdata is displayed
        > sudo dpkg-reconfigure tzdata

   3. For the Geographic area, use the arrow keys to select
        `None of the above`
   4. Press Enter
   5. For the Time zone, use the arrow keys to select UTC
   6. Press Enter

### *Install Apache*

   1. Install Apache
        > sudo apt-get install apache2
   2. Open a browser
   3. Go to the URL: <http://18.237.100.85>
   4. If Apache is installed properly, the Apache2 Ubuntu Default Page
        will be displayed

### *Install Python mod_wsgi*

   1. Install the mod_wsgi package
        > sudo apt-get install libapache2-mod-wsgi python-dev
   2. Enable mod_wsgi
        > sudo a2enmod wsgi
   3. Restart Apache
        > sudo service apache2 restart

## Install and configure PostgreSQL

### *Install PostgreSQL*

   1. Install PostgreSQL with the command
        > Run $ sudo apt-get install postgresql

### *Do not allow remote connections*

   1. Make sure PostgreSQL does not allow remote connections
   2. Go to the `/etc/postgresql/9.5/main/` directory
   3. Edit the `pg_hba.conf` file
        > sudo nano pg_hba.conf
   4. Check to make sure it looks like this:

    # Database administrative login by Unix domain socket
    local   all             postgres                                peer

    # TYPE  DATABASE        USER            ADDRESS                 METHOD

    # "local" is for Unix domain socket connections only
    local   all             all                                     peer
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5
    # IPv6 local connections:
    host    all             all             ::1/128                 md5

### *Create a new database catalog*

   1. Switch to PostgreSQL defualt user postgres
        > sudo su - postgres
   2. Connect to PostgreSQL
        > psql
   3. Create user catalog with LOGIN role
        > CREATE ROLE catalog WITH PASSWORD 'password';
   4. Allow user to create database tables
        > ALTER USER catalog CREATEDB;
   5. Create database
        > CREATE DATABASE catalog WITH OWNER catalog;
   6. Connect to database catalog
        > \c catalog
   7. Revoke all the rights
        > REVOKE ALL ON SCHEMA public FROM public;
   8. Grant access to catalog
        > GRANT ALL ON SCHEMA public TO catalog;
   9. Exit psql
        > \q
   10. Exit user postgres
        > exit

### *Create new PostgreSQL user called catalog to access the catalog database*

   1. Switch to PostgreSQL defualt user postgres
        > sudo su - postgres
   2. Connect to PostgreSQL
        > psql
   3. Create user catalog with LOGIN role
        > CREATE ROLE catalog WITH PASSWORD 'password';
   4. Allow user to create database tables
        > ALTER USER catalog CREATEDB;
   5. Create database
        > CREATE DATABASE catalog WITH OWNER catalog;
   6. Connect to database catalog
        > \c catalog
   7. Revoke all the rights
        > REVOKE ALL ON SCHEMA public FROM public;
   8. Grant access to catalog
        > GRANT ALL ON SCHEMA public TO catalog;
   9. Exit psql
        > \q
   10. Exit user postgres
        > exit

## Install git

   1. Install Git with this command
        > sudo apt-get install git-core

## Deploy the Item Catalog project

### *Clone and setup your Item Catalog project from the Github repository*

   1. Go to the `/tmp` directory
   2. Download the Item Catalog Project from Git.
        > git clone {RELEVANT_URl}
   3. Create dictionary
        > mkdir /var/www/catalog
        > mkdir /var/www/catalog/catalog
   4. Go to the `/var/www/catalog/catalog` directory
        > cd /var/www/catalog/catalog
   5. Copy the contents to the `/var//www/catalog/` directory
        > sudo cp {CREATED_PROEJCT_FOLDER} /var/www/catalog/catalog/
   6. Go to the `/var/www/catalog/catalog` directory
   7. Change file `views.py` to `__init__.py`
        > mv `views.py` `__init__.py`
   8. Edit the `__init__.py` file
        > sudo nano `__init__.py`
   9. Go to the line: `app.run(host='0.0.0.0', port=8000)`
   10. Change the line to `app.run()`

### *Install the necessary software to allow the Python program to run*

    > sudo apt-get install python2.7
    > sudo apt-get install python-setuptools
    > sudo pip install werkzeug==0.8.3
    > sudo pip install flask==0.9
    > sudo pip install Flask-Login==0.1.3
    > sudo pip install pyopenssl
    > sudo pip install requests
    > sudo apt-get install postgresql
    > sudo easy_install sqlalchemy

### *Get the domain name for the AWS server site*

   1. Go to the link: `https://mxtoolbox.com/ReverseLookup.aspx`
   2. For the IP Address field, enter your public IP Address
   3. Press the `Reverse Lookup` button
   4. Make a note of your PUBLIC AWS SERVER DOMAIN NAME

### *Setup the virtual host*

   1. Create file that will be used for the virtual host
        > sudo touch `/etc/apache2/sites-available/catalog.conf`
   2. Enter these line into the file

~~~
        <VirtualHost *:80>
                ServerName x.x.x.x
                ServerAdmin admin@x.x.x.x
                ServerAlias {PUBLIC AWS SERVER DOMAIN NAME}
                WSGIScriptAlias / /var/www/catalog/catalog/catalog.wsgi
                <Directory /var/www/catalog/catalog/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog/catalog/static
                <Directory /var/www/catalog/catalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/catalogerror.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/catalogaccess.log combined
        </VirtualHost>
~~~

### *Enable the virtual host*

   1. Enable the virtual host
        > sudo a2ensite catalog
   2. Restart Apache
        > sudo service apache2 reload

### *Configure .wsgi file*

   1. Create the wsgi file used by the catalog application
        > sudo touch /var/www/catalog/catalog/catalog.wsgi
   2. Add the following to the file

~~~
        #!/usr/bin/python
        import sys
        import logging
        logging.basicConfig(stream=sys.stderr)
        sys.path.insert(0,"/var/www/catalog/catalog/")

        from catalog import app as application
        application.secret_key = 'super_secret_key'
~~~

   3. Restart Apache to use the changes
        > sudo service apache2 reload

### *Update the Item Catalog application to use the Postgres database*

   1. Go to `/var/www/catalog/catalog/` directory
   2. Edit the `models.py` file
        > sudo nano `models.py`
   3. Go to the line
        > `engine = create_engine('sqlite:///categoryitem.db')`
   4. Change the line to:
        > `engine = create_engine(`
            `postgresql://catalog:mypass@localhost/catalog')`

### *Disable defualt Apache page*

   1. Run this command to disable the default Apage page
        > sudo a2dissite 000-defualt.conf
   2. Restart the Apache server
        > sudo service apache2 reload

### *Create the database schema and populate the catalog database*

   1. Run the command to create the database schema
        > sudo python `models.py`
   2. Run the command to populate the catalog database
        > sudo python `lotsofcategoryitems.py`
   3. Restart the Apache server to use the changes
        > sudo service apache2 reload

### *Go to the browser and view the Item Catalog application*

   1. Open the browser
   2. Go to the URL: {PUBLIC AWS SERVER DOMAIN NAME}
   3. View the Item Catalog Application and enjoy
   4. If internal errors occur, check the Apache error files located
        `/var/log/Apache2/`
