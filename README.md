# Linux Server Configuration

## About

This is the sixth project for the Udacity Full Stack Nanodegree. This project involves taking a baseline installation of Linux on a virtual machine and preparing it to host web applications. This includes installing updates, securing the server from attacks, and installing / configuring web and database servers.


# Server Info

- **IP Address:** 13.232.98.88
- **Port:** 2200

- http://13.232.98.88.xip.io/
#### Step-1

##### Software Requirements:

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
#### Step-2

##### Server Configuration:

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

Then load the privatekey and a new terminal will open press no.
Login as ubuntu

4. Connect to grader by using the following command:	

		ssh -i path/to/privatekey -p 2200 grader@ip address				
		
5. Creating grader User:					

		ubuntu@ip-172-26-0-113:~$ sudo apt-get update
	
		ubuntu@ip-172-26-0-113:~$ sudo apt-get upgrade
	
		sudo adduser grader
	
 Follow the following steps:			

	sudo nano /etc/sudoers			
	
Grant sudo permission to grader			

	grader  ALL=(ALL:ALL) ALL		
 
6. Generate ssh key pair for grader:

open command prompt, use command ssh-keygen

press enter, again enter, again enter, again enter.This will generate public and private ssh keys

Now go to local disk C, and User, your computerName, open .ssh file.

You will see id_rsa and id_rsa.pub.

open id_rsa.pub and keep aside.

7. We have to create a ssh key pair for grader	

Open new terminal/command prompt and type 	

	ssh-keygen					
	
This will generate public and private ssh keys which is saved to .ssh folder

8. Change ubuntu user to grader

		sudo su - grader
	
9. Create a new directory .ssh and new file authorized_keys in that directory

		mkdir .ssh
	
        	sudo nano .ssh/authorized_keys
	
10. Permissions:	

		chmod 700 .ssh	
	
		chmod 644 .ssh/authorized_keys
    
		sudo service ssh restart
    
11. Now from your log in to grader with private key generated

    		ssh -i .ssh/id_rsa grader@ipaddress 
		

12. Changing SSH port to 2200
 
-- use command to edit sshd config file. grader@ip-172-26-0-113:~$ sudo nano /etc/ssh/sshd_config
 
-- Now in this,change port from 22 to 2200.demo

-- restart server ssh using command, grader@ip-172-26-0-113:~$ sudo service ssh restart

-- Before Logging using ssh add custom TCP port 2200 and add custom UDP port 123 under lightsail firewall in 
   networking tab in lightsail instance console.demo
   
    	ssh -i .ssh/id_rsa -p 2200 grader@ipaddress
    
Note:Important thing is disabaling ssh login as root
		
	sudo nano /etc/ssh/sshd_config

make change PermitRootlogin no
    
13. Update 

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
	     
#### Step-3

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
 
Open the given below source 

https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
https://github.com/vyshnavimuthumula/Linux-server

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

#### Step-4

Configure and Enable a New Virtual Host, then type the following code in the shell

<VirtualHost *:80> ServerName mywebsite.com ServerAdmin admin@mywebsite.com WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi <Directory /var/www/FlaskApp/FlaskApp/> Order allow,deny Allow from all Alias /static /var/www/FlaskApp/FlaskApp/static <Directory /var/www/FlaskApp/FlaskApp/static/> Order allow,deny Allow from all ErrorLog ${APACHE_LOG_DIR}/error.log LogLevel warn CustomLog ${APACHE_LOG_DIR}/access.log combined

    sudo a2ensite FlaskApp

.wsgi file

     sudo nano flaskapp.wsgi
     
Then type the below code

#!/usr/bin/python import sys import logging logging.basicConfig(stream=sys.stderr) sys.path.insert(0,"/var/www/FlaskApp/") from FlaskApp import app as application application.secret_key = 'Add your secret key'

press ctrl+x and y

#### Step-5

Go to google console and change homeurl to

http://ipaddress.xip.io\callback

http://ipaddress.xip.io\gconnect

http://ipaddress.xip.io\login

#### FINAL STEP:

    sudo service apache2 restart
    
Then go to webbrowser and type your ipaddress finaly we get our project.

**** My server Details

   Server static IP Address 13.232.98.88
**** Grader Key

-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA/ahYmdAHYA1zJiv4iJfArM4H4og/OC9EIoJdrsmaW7cZUUMT
Lq+GjFV3SJ7iQLrMK5Tg9SglQKRtMiZnQCsQr5aUqrPPvczWlTL9QSINAk1W7yt0
qPUZAV0atH8GcDynCfZ6PRUzONS+OqJVJddL8/Qqb7ypdsMGW6EUwo1YuuPHjdiZ
DV5fIOxwyyLIA3GUqPUp7KrL+HvLBbgVkzxKRKcdFPwmEq7emwSLzdqVjn7y65Jj
xLPkx/XVo/3DG92ym96mrcJANVoQ2ruFXAhp7lU0oU2tmDBGkl+WDcH7y+nwztAG
AiXqkALVrW+ZC67lpf3+O8Eo/OyrxvVY3uiwdQIDAQABAoIBAH2ZlT2cT3qVTlYx
YaApHEO0xRy7gCpO5Tr9OGwq8V7GnCerwdzVFxd33le8LKYGmMBfuMBLv55xjIxP
jcKtUFbRhg28eXou6nX4SISu2qgwKYLGDr72lgoh0u5bE5IRxlhdjouear2SQhuA
dA8Hu1kxpq2rSnI/AW/vo/rtyGjJEpLhJeV2bFKLGqV19BE923CxuoS9Lh9T5DJZ
d2dOxLgbaw03Hr/j1GzFuRS82dpbgSRrrsRw4YKqtnaA6Qdx6jE8010FPXB9S4J3
Z3apAzxrL10THWFOs1bxwZVZoEJNlTNFm61sPigGNKrMUJiE/pgOp5MMKliaAaHP
XO6b0eECgYEA/85MjpIcnDJkVcubIl8xy9DkryX+xumoW/3xX8Xhc+UHiY0kUkPc
JBAdaL/sgpxndku3P9Z+B9OmvJwXSGhKp+pQP8mZLNjU2VlW/8hAtJQSn1jMFgPt
1wrfC/fI/MoM7bJSIItaEnLGFP/0cooq5+MLBJ7xtKDrJSPpv3QgBf0CgYEA/dmh
MVA0gIYitYELMmBl5d7Y8M4K4ECo0nOENZpQFbCuCwqNMQmxXUi8+vGsOrIeVpfR
leQxwuSr7KmWKLfTr4NwJsWsXZbkTwITYsd4xhLOyO/pOVH6HyUivLkqvPp7C3uD
m7YUu3fCNc+PllTKCwWvweFhy/+DwrQsBjxWIdkCgYBMJ+fk3h0EZ4A1hqtF3V9e
1W7vsfka0P9de8m7gJbxQPMwgUOZ9jf4yI9o2xKXg+bNchc5OytEOz+9kR7hYKMx
QHHpu6QNlPQxTQa4ma6h1B+DLxV7TGonhkYHMxq0H5cfwOHwbGxBZ8gPAnCNFRNW
++IQ2x0McIfxA7MYW4MZJQKBgQDswr+SI/Fj8jeLPBl6WeiQNoH2TuZb9FLBPpaP
/CY3pLsfdy7rDtRLYh1InIF7mUeskhsbh2NWGDu2FxIDVjjs2VWQBAxYmfTFL/Vu
ywb9DuupBAJtwOTdiaBVjwqqiaCbvA6q+29ozjDoSXftyZVMJHiiBxlU0DNPNQZe
poXbOQKBgCUp1wedPih9pQZWtoR3G6DParesurn/a+Q++AQGo6ht6VLafpn7aKR5
xYj30ekP+i2RzoXk509A4SBdBAzRsgWyzwzdU+9w/soYK+6PBl+O0jpdyi/UygoD
8cBVsZEXnsuzk53VrjhG1ZIdRIJiM2XNwJ4E/6SYWPIrt6IYh8UB
-----END RSA PRIVATE KEY-----


Grader Password

	unix
   
###### Deploy the Item Catalog project.

    http://13.232.98.88
    
## Resources


https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04

https://help.ubuntu.com/community/PostgreSQL
