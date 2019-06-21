# Linux Server Configuration and Web Application Deployment

## Udacity Fullstack Nanodegree Final Project
The goal of this Capstone project is to deploy a web application built from a previous project to a publicly accessible Ubuntu linux server. I use AWS Lightsail instance as the Virtual linux server and Ubuntu as its operating system for this project. 

## Project Detail

* Public IP Address: 54.218.238.1
* Access the Virtual Linux Server by ```ssh -i LightsailUdacityLinux.pem ubuntu@54.218.238.1``` (LightsailUdacityLinux.pem is the private key downloaded from the Lightsail instance after its creation)
* It is a good practice to update&upgrade all softwares in order to keep the system secure. 
    * ```sudo apt-get update```
    * ```sudo apt-get upgrade```



## References

* [SSH port](https://www.unixtutorial.org/ssh-port)
