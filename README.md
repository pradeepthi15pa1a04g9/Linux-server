# Linux Server Configuration

## About

This is the sixth project for the Udacity Full Stack Nanodegree. This project involves taking a baseline installation of Linux on a virtual machine and preparing it to host web applications. This includes installing updates, securing the server from attacks, and installing / configuring web and database servers.


# Server Info

- **Public IP:** 34.210.194.203
- **Port:** 2200

- http://34.210.194.203/

## Getting Started

This project uses [Amazon Lightsail](https://amazonlightsail.com/) to create a Linux server instance.

1. Get your server.

    - Start a new Ubuntu Linux server instance on Amazon Lightsail. 
        
        * Log in!
        * Create an instance
        * Choose an instance image: Ubuntu (OS only)
        * Choose your instance plan (lowest tier is fine)
        # Linux Server Configuration

## About

This is the sixth project for the Udacity Full Stack Nanodegree. This project involves taking a baseline installation of Linux on a virtual machine and preparing it to host web applications. This includes installing updates, securing the server from attacks, and installing / configuring web and database servers.


# Server Info

- **Public IP:** 34.210.194.203
- **Port:** 2200

- http://34.210.194.203/
#### Software Requirements:
AWS account with lightsail service activated.

1)Python Pip

2)Postgres

3)httplib2

4)SQLAlchemy

5)Apache2

6)Flask

7)Virtualenv

8)Requests

9)Oauth2client

## Process:

This project uses Amazon Lightsail(https://amazonlightsail.com/) to create a Linux server instance.

1. Get your server.

   - Open your lightsail amazon account and follow the give steps
        
        * Log in!
        * Create an instance
        * Choose your instance plan 
        * Wait for startup
3. Download putty.exe,puttygen.exe in putty.org 		

-- Open Putty.exe and give staticIP of instance as hostname.	

select file downloaded from shh-keys eg: LightsailDefaultPrivateKey-ap-south-1.pem
click ok.then save private, yes, give some name.. save on desktop.then close it.						
now you will see a .ppk file on desktop.	

From left tree, click on SSH. after, Auth. You will see browser button.								
click on browser, and select .ppk file and click on open.									
press No. username ubuntu	

4. Create static ip in lightsail and download it.

5. Open putty enter your ip and add private key address.	

6. Connect to grader by using the following command:	

	ssh -i path/to/privatekey -p 2200 grader@ip address				
	
7. Creating grader User:					

	ubuntu@ip-172-26-0-113:~$ sudo apt update 	
	
	sudo adduser grader
 Follow the following steps:			

	sudo nano /etc/sudoers			
	
Grant sudo permission to grader			

	grader  ALL=(ALL:ALL) ALL		
 
8. Generate ssh key pair for grader:

open command prompt, use command ssh-keygen

press enter, again enter, again enter, again enter.This will generate public and private ssh keys

Now go to local disk C, and User, your computerName, open .ssh file.

You will see id_rsa and id_rsa.pub.

open id_rsa.pub and keep aside.
9. We have to create a ssh key pair for grader	

Open new terminal/command prompt and type 	

	ssh-keygen					
	
This will generate public and private ssh keys which is saved to .ssh folder

10. Change ubuntu user to grader

	sudo su - grader
	
11. Create a new directory .ssh and new file authorized_keys in that directory

	mkdir .ssh
	
        sudo nano .ssh/authorized_keys
	
12. Permissions:	

	chmod 700 .ssh	
	
	chmod 644 .ssh/authorized_keys
    
	sudo service ssh restart
    
12. Now from your log in to grader with private key generated

    ssh -i .ssh/id_rsa grader@ipaddress 

13. Changing SSH port to 2200
 
-- use command to edit sshd config file. grader@ip-172-26-0-113:~$ sudo nano /etc/ssh/sshd_config
 
-- Now in this,change port from 22 to 2200.demo

-- restart server ssh using command, grader@ip-172-26-0-113:~$ sudo service ssh restart

-- Before Logging using ssh add custom TCP port 2200 and add custom UDP port 123 under lightsail firewall in 
   networking tab in lightsail instance console.demo
   
    ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
    
14. Update 

    - Update all currently installed packages.
    
            sudo apt-get update
            sudo apt-get upgrade
            
        
    - Configure the Uncomplicated Firewall (UFW):
        
            sudo ufw allow 2200/tcp
            sudo ufw allow 80/tcp
            sudo ufw allow 123/tcp
            sudo ufw enable
            sudo ufw status
    - Changing time Zone
     
            sudo dpkg-reconfigure tzdata
  ##### Installation Process
            sudo apt-get install apache2		

             sudo apt-get install python-setuptools libapache2-mod-wsgi

             sudo a2enmod wsgi

             cd /var/www

             sudo mkdir FlaskApp

             sudo apt-get install git

             sudo apt-get install python-pip virtualenv

             sudo virtualenv venv

             sudo pip install Flask

             sudo pip install postgresql oauth2client httplib2 requests psycopg2

             cd /var/www/FlaskApp
 
             sudo rm -r FlaskApp
##### Step-3

 In that directory clone your github repository

        sudo git clone 'https://github.com/username/filename.git'
        mkdir FlaskApp
        cd FlaskApp
        ls
        sudo mv finalproject.py __init__.py
        ls
 Then we get all files in that directory.
 
 Changing database in both database_setup.py and __init__.py

        sudo nano database_setup.py
 
Open

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps

Use this line to Replace database connection in init.py and database_setup.py

    engine = create_engine('postgresql://catalog:password@localhost/catalog')

PROJECT_ROOT = os.path.realpath(os.path.dirname(file)) json_url = os.path.join(PROJECT_ROOT, 'client_secrets.json')
CLIENT_ID = json.load(open(json_url))['web']['client_id']

Replace client_secrets.json with json_url in scriptfile

    sudo apt-get install postgresql

    sudo su - postgres

    psql
Now we have to create user

     CREATE USER catalog WITH PASSWORD 'password';
CREATE USER

    ALTER USER catalog CREATEDB;
ALTER USER

    CREATE DATABASE catalog WITH OWNER catalog;
DATABASE CREATED

Move to database catalog

\c catalog  

    REVOKE ALL ON SCHEMA public FROM public;
REVOKE

    GRANT ALL ON SCHEMA public TO catalog;
\q and type exit

Change the database connection in both database_setup.py and init.py as

    engine = create_engine('postgresql://catalog:password@localhost/catalog')

##### Step-4

Configure and Enable a New Virtual Host, then type the following code in the shell

<VirtualHost *:80> ServerName mywebsite.com ServerAdmin admin@mywebsite.com WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi <Directory /var/www/FlaskApp/FlaskApp/> Order allow,deny Allow from all Alias /static /var/www/FlaskApp/FlaskApp/static <Directory /var/www/FlaskApp/FlaskApp/static/> Order allow,deny Allow from all ErrorLog ${APACHE_LOG_DIR}/error.log LogLevel warn CustomLog ${APACHE_LOG_DIR}/access.log combined

    sudo a2ensite FlaskApp

.wsgi file

     sudo nano flaskapp.wsgi
     
Then type the below code

#!/usr/bin/python import sys import logging logging.basicConfig(stream=sys.stderr) sys.path.insert(0,"/var/www/FlaskApp/") from FlaskApp import app as application application.secret_key = 'Add your secret key'

press ctrl+x and y

##### Step-5

Go to google console and change homeurl to

http://ipaddress.xip.io\callback

http://ipaddress.xip.io\gconnect

http://ipaddress.xip.io\login

##### FINAL STEP:

    sudo service apache2 restart
    
Then go to webbrowser and type your ipaddress finaly we get our project.

**** My server Details

   Server static IP Address 13.232.39.180
**** Grader Key

-----BEGIN RSA PRIVATE KEY----- MIIEogIBAAKCAQEAsjQ6MwF5H7/yGlQY+EVcK+atb3/oG7a+MavG2N2EvdOM3iSX 6GjmckT1QljOxpRlLZGJ5H8aRgYitXR0xYdsHJa/AYfGp4i/k6+/U7bkvxSAXIE9 DQrTrE3FyKFcrNkwFs3aeOYwslMYAbdw97cTLwsQdjl+J+J+zVUh/KQsBHtsNwrU 7vplozXR0LNDXv6sddu7fGv5w1oJ6hEMTiI3qZd1KBZI9sjP4PZ7UkQ539etZ6Vg 4cN3WssUlOrUNYM7MIgSXmFMVJiPL8KxY4SzS+ZF/R9ZFDezKbREPi71R1/jKivL YM5QhNatCCahKqN+4I02esTbO52jEvs1mfGsKwIDAQABAoIBAAe6UBPKKpB/6GXP 481QZLDarga5yzz4bcMFqffZk1oQBHnVqGjBs8ycxO39n+nooYKaXxpzkJYcygCI bk/qkXuj5eCRHMJDIdursWZV9hF7OB3K1PTt1UQRk1Qh+zzbpkQ25RR9FvuEsvPQ GqwDWmed2TbnQ1tDbTBGUtT74ZTIGty35lPubvAE13dbBI2aVWGKjJtfRxDhpBQf zyZzl4iKx39MHTRoJVhnJ5eNVJv1BYMB+7FDg+zIgDAOj7u5eQtWnxDkWZH7FA+O FUhqTtXOl5Juw2eK6S/WG/lYlpOyVJibenvyCsqOHs1LIYS0kxMM2z6ZgEEtbe58 xgVNPqECgYEA3sJU3xSdKb47IE/b0fJzDhYjmk7gSyLk/bqyE//5bK79A6MXA9yz JjJwDgMI+odb9FrHCR2UIGzsyIXqTx9B0bF2e2sqaPUbra7a/+7O6I33i9vipjZU hzAWU1kV9zVzmC1U0+lFtviynMypz65XFVv5n8EeZ934C6dFjuyegZMCgYEAzMvW S0L3MCTGInFN/j/JgNb873ZWrVnz+GOgQFgAQmjUcyJOrEifgOrU+ZYhHEeU/gwD ZEPOTyfXAOJBkxz+JSUFe5xAkyygL/51woaVEkDGyeQiaSfnEckJ3u2xxUurxWiQ ycJuOxetwnVSxOOy8Kr0qmk75vbOkR9wyUKPKgkCgYAluM7agAklOnuUuzFEWkQ1 jHY2+UhuMNiKRwVE8cHxL6jU5tdM5iDIRR5IoSbyFd3ygTTXTFT7MLbgNh05jNd+ hQjFWZ5y657mSIf5cx1CsFfNLU0yTF0AD5qYPqvDkx+iE3sb75LIq1DD0Lyo2KMS kOKytOdLO4F3p7nVvCgTVQKBgCMGGTf10+Bf6aKqTfRVZFisa8VoL5ql75tjLlzS r/irhOnLzDiakuyxPIsSqcb0Vv67fzj+f6H55kM4bo6CPtSLaEyjhEenMh4DHpCO A6CDg3uzkE77jAD2qMF/VQ+wyUeRgnF+1us0OXswJV+WsVuHYSBjruLpApq/DcLd py5BAoGAYqPrT05A522n3cuFgVCJKKeOBVv3WZQ0pscOic5fvN551IOiQ3S3TsGv ex4pTcxTvHRquF0RspeL4s9v//1DQibDvrmvVpU9iYEJ9PV9Lv5+XRNK0Kzlv6uk HE0hv/m0hKjUubCb8sCFe1vIum34I3dtWQB0YChQjp8F7rFP5oA= -----END RSA PRIVATE KEY-----

Grader Password
unix

OUTPUT:
CLICK HERE http://13..xip.io/

   
        https://help.ubuntu.com/community/PostgreSQL

6. Deploy the Item Catalog project.

    http://34.210.194.203/
For error checking :

    sudo nano /var/log/apache2/error.log
    
## Resources


https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

https://help.ubuntu.com/community/PostgreSQL
            
    - Install and configure Apache to serve a Python mod_wsgi application.
    
        ngnix is used instead of Apache
        
        configuration steps can be found here 
        
        https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04
        

    - Install and configure PostgreSQL:
    
            sudo apt-get install postgresql

5. Do not allow remote connections
    
    Create a new database user named catalog that has limited permissions to your catalog application database.
    
        https://help.ubuntu.com/community/PostgreSQL

6. Deploy the Item Catalog project.

    http://34.210.194.203/
    
## Resources


https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

https://help.ubuntu.com/community/PostgreSQL
