# Linux Server Configuration and Web Application Deployment

## Udacity Fullstack Nanodegree Final Project
The goal of this Capstone project is to deploy a web application built from a previous project to a publicly accessible Ubuntu linux server. I use AWS Lightsail instance as the Virtual linux server and Ubuntu as its operating system for this project. 

## Project Detail

* Public IP Address: 54.218.238.1
* Access the Virtual Linux Server by ```ssh -i LightsailUdacityLinux.pem ubuntu@54.218.238.1``` (LightsailUdacityLinux.pem is the private key downloaded from the Lightsail instance after its creation)
* It is a good practice to update&upgrade all softwares in order to keep the system secure. 
    * ```sudo apt-get update```
    * ```sudo apt-get upgrade```
    
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
   * Next, we want to grant the grader the ability as if it were a superuser. Technically speaking, this is called "granting grader a sudo permission". We use the command ```
   * (_optional_) ```sudo apt install finger``` and ```finger grader``` to confirm if the new user and its information are correctly updated.




## References

* [SSH port](https://www.unixtutorial.org/ssh-port)
