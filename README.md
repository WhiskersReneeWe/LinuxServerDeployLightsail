# Linux Server Configuration and Web Application Deployment

## Udacity Fullstack Nanodegree Final Project
The goal of this Capstone project is to deploy a web application built from a previous project to a publicly accessible Ubuntu linux server. I use AWS Lightsail instance as the Virtual linux server and Ubuntu as its operating system for this project. 

## Project Summary
 * Ubuntu Server Username: grader
 * SSH port: 2200
 * IP address: 54.218.238.1
 
 
## Project Detail

* Public IP Address: 54.218.238.1
* Access the Virtual Linux Server by ```ssh -i LightsailUdacityLinux.pem ubuntu@54.218.238.1``` (LightsailUdacityLinux.pem is the private key downloaded from the Lightsail instance after its creation)
* It is a good practice to update&upgrade all softwares in order to keep the system secure. 
    * ```sudo apt-get update```
    * ```sudo apt-get upgrade```
    * ```sudo apt-get dist-upgrade``` (if some packages are kept back)
    
* Configure the Firewall to support our applications and ports needed for this project:
   * Step 0: First and foremost, ```sudo ufw allow 2200/tcp``` and create custom port on Lightsail instance.
   * Step 1: ```sudo nano /etc/ssh/sshd_config``` Change port 22 to 2200 as required for this project.
   * Step 2: ```sudo nano /etc/ssh/ssh_config``` Change port 22 to 2200 as required for this project.
   * Step 3: ```sudo service ssh restart``` and ```sudo service sshd restart```
   * Step 4: ```sudo ufw allow 2200/tcp```  to allow the server listen on port 2200. Meanwhile, add a cutom port 2200 on Lightsail Firewall setup section.
   * Step 5:  ```sudo ufw deny 22``` once we are able to SSH into the server on on port 2200.
   * Step 6: SSH into the server on port 2200. ``````ssh -i LightsailUdacityLinux.pem ubuntu@54.218.238.1 -p 2200 ```
   
 * Add a new user - grader, and grant it access.
   * ```sudo adduser grader``` (Create an account for a new user named grader)
   * (_optional_) ```sudo apt install finger``` and ```finger grader``` to confirm if the new user and its information are correctly updated.
   * Next, we want to grant the grader the ability as if it were a superuser. Technically speaking, this is called "granting grader a sudo permission or root priviledges". We use the command ```sudo ls /etc/sudoers.d```. This lets you see the current users who have sudo priviledges. 
   * ```sudo visudo``` and add ```grader  ALL=(ALL:ALL) ALL``` below the existing line -- ```root  ALL=(ALL:ALL) ALL```
   
   * ```exit``` to get back to the local terminal.
   * Now, we need a Public/Private keypair in order for grader user to log into the Server.
   * ```ssh-keygen```. We use the generated public key to configure over the server so that Grader can log into the server. 
   * ("Key" Step )Copy the public key.
   * SSH into the Server as Ubuntu user. Then, cd /home/grader to the grader's home directory.
   * Create a new directory called .ssh using ```sudo mkdir .ssh``` and change its ownership to grader: ```sudo chown grader:grader  .ssh/ ```
   * cd into /home/grader/.ssh folder. Then, paste the public key from (Key Step) above into a new file named 'authorized_keys'. I.e.,
   Use this command - ```sudo nano authorized_keys```, then just paste that public key in it. Save and exit.
   * Type ```cat authorized_keys``` to see if the public key is successfully pasted in it.
   * Exit. Now, you should be at your local terminal.
   
   
* Log into the Server as User: grader
   * Now that the server has the public key generated for grader. All we need to do to log in as grader is to use the private key generated. Use this command -- ```ssh grader@54.218.238.1 -i graderKey -p 2200```. The logic here is that the private key can be paired up with the public key that exists in the server for user Grader. This is the verification process for user:grader to log in.
   * Configure the UTC time zone as the user:grader by ```sudo dpkg-reconfigure tzdata```. You need to choose the appropriate time zone from the options.

* Install and configure Apache to serve a Python mod_wsgi application
   * ```sudo apt-get install apache2``` to install Apache2.
   * ```sudo apt-get install libapache2-mod-wsgi-py3``` to install Python 3 mod_wsgi package. My webapp is written in Python3. Note, this command automatically enables wsgi.
   

* Install and configure PostgreSQL
  * Similarly, we install PostgreSQL database server by  ``` sudo apt-get install postgresql```.
  * Log in as Postgres user to create `itemcatalog` database by -- ```CREATE DATABASE itemcatalog```.
  * Create a PostgreSQL role and grant it ability to full permission 
   * ```CREATE ROLE ubuntu WITH LOGIN PASSWORD 'A_PASSWORD_OF_YOUR_CHOICE';``` 
   * ```grant all privileges on database itemcatalog to ubuntu;```
   * Clone The Webapp from your Github Repository into a folder /var/www/catalog (create it when logged in as grader user).
   * While logged in as a grader user, cd into /var/www/catalog. Then type the Command: ``` sudo git clone https://github.com/WhiskersReneeWe/Item-Catalog.git ./```. This will copy the webapp from your personal Github repo to the Grader account.
   * ```sudo nano yourdatabase.py``` to edit one line with  ```engine = create_engine('postgresql://ubuntu:PASSWOR_OF_YOUR_CHOICE@localhost/itemcatalog')```
   * ```sudo python db_setup.py```
   * Very similarly, * ```sudo nano yourdatabase_initialdata.py``` to edit one line with  ```engine = create_engine('postgresql://ubuntu:PASSWOR_OF_YOUR_CHOICE@localhost/itemcatalog')```
   * ```sudo python db_initialdata.py```
   * Now, you should have populated the newly created database.
   * Exit to your local terminal.
   * SSH into the server as grader. 
   
   
 * Get the Webapp ready
   * Log into the server as Ubuntu user.
   * ```sudo nano yourwebapp.py``` to edit one line with  ```engine = create_engine('postgresql://ubuntu:PASSWOR_OF_YOUR_CHOICE@localhost/itemcatalog')```
   * Edit the gconnect() endpoint where 
       ```oauth_flow = flow_from_clientsecrets('/var/www/html/client_secret.json', scope='') ```
   * Change the path to client_secret.json accordingly.
   * Copy everything into path -- /var/www/html
   * Create a wsgi file. I named this file myapp.wsgi
      
      ```import sys
       sys.path.append("/var/www/html")
       from app import app as application
       application.secret_key = 'supersecretkey'
       ```
   * Update all necessary packages (I used pip3):
     ```sudo pip3 install flask 
        sudo pip3 install packaging 
        sudo pip3 install oauth2client 
        sudo pip3 install flask-httpauth
        sudo pip3 install sqlalchemy 
        sudo pip3 install flask-sqlalchemy 
        sudo pip3 install requests
        sudo pip3 install psycopg2
      ```

* Go to: http://54.218.238.1.xip.io/ to check out the app.

  





## References

* [SSH port](https://www.unixtutorial.org/ssh-port)
* [Awesome SSH Daemon Tutorial](https://www.digitalocean.com/community/tutorials/how-to-tune-your-ssh-daemon-configuration-on-a-linux-vps)
* [grant user sudo priviledges](https://github.com/boisalai/udacity-linux-server-configuration)
