
# Devops Project-8

## LOAD BALANCER SOLUTION WITH APACHE

#### CONFIGURE APACHE AS A LOAD BALANCER

* Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb.

* Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.

![new53](https://user-images.githubusercontent.com/115363604/224460672-c36d26ef-fd6a-4d20-a027-e8b1aeaa1bc2.png)

* Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:
  - sudo apt update
  - sudo apt install apache2 -y
  - sudo apt-get install libxml2-dev

* Enable following modules:
  - sudo a2enmod rewrite
  - sudo a2enmod proxy
  - sudo a2enmod proxy_balancer
  - sudo a2enmod proxy_http
  - sudo a2enmod headers
  - sudo a2enmod lbmethod_bytraffic

* Restart apache2 service
  - sudo systemctl restart apache2
  - sudo systemctl status apache2 ( To make sure apache2 is up and running )
  
![new54](https://user-images.githubusercontent.com/115363604/224460801-bec4d829-1fdd-4b36-9479-9b66040d4517.png)

* Configure load balancing.
  - sudo vi /etc/apache2/sites-available/000-default.conf
  
![new55](https://user-images.githubusercontent.com/115363604/224460947-dff184f1-f045-403f-9694-be0d5a80a650.png)

* Restart apache server
  - sudo systemctl restart apache2
  - sudo systemctl status apache2
  
![new56](https://user-images.githubusercontent.com/115363604/224461029-01e7783d-d769-4d46-ba50-18f8b1bdba8b.png)

* Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:
  - http://Load-Balancer-Public-IP-Address-or-Public-DNS-Name/index.php
  
![new57](https://user-images.githubusercontent.com/115363604/224461334-4ed9710d-4fdb-4066-ba88-da5d0262093e.png)

* Spin up my *webserver2* and run the commands:
  -	sudo setenforce 0
  -	sudp vi /etc/sysconfig/selinux ( change it to disabled )
  -	sudo systemctl start httpd
  -	sudo systemctl status httpd
  
* Go back and reload the project-8-apache-lb server.

![new58](https://user-images.githubusercontent.com/115363604/224461471-3241fdc8-bc1b-4c39-b7e2-64233d0d0742.png)

![new59](https://user-images.githubusercontent.com/115363604/224461473-af8bda88-b0c6-43d8-8752-2a60cd0d66af.png)

* Open two ssh/Putty consoles for both Web Servers and run following command:
  - sudo tail -f /var/log/httpd/access_log
  
![new60](https://user-images.githubusercontent.com/115363604/224461616-ee108ccd-f33f-42ed-845b-2c0394303ab1.png)

![new61](https://user-images.githubusercontent.com/115363604/224461619-952d5583-bb68-486d-bf1b-69b1317a3dd7.png)

* Try to refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers receive HTTP GET requests from your LB – new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.

* If you have configured everything correctly – your users will not even notice that their requests are served by more than one server.

#### Optional Step – Configure Local DNS Names Resolution.

* Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management.
* What we can do, is to configure local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.

* Open this file on your LB server.
  - sudo vi /etc/hosts

* Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers.
  - WebServer1-Private-IP-Address Web1
  - WebServer2-Private-IP-Address Web2
  
![new62](https://user-images.githubusercontent.com/115363604/224461904-ddbb2d2b-b73f-41f4-8678-f06a219d173b.png)

* Now you can update your LB config file with those names instead of IP addresses.
  - Sudo vi /etc/apaches/sites-avaliable/000-default.conf

![new63](https://user-images.githubusercontent.com/115363604/224461910-2ccd88d1-e135-4794-abea-a9d137cdcb4f.png)

* You can try to curl your Web Servers from LB locally curl http://Web1 or curl http://Web2 – it shall work.
  - curl http://Web1
  - curl http://Web2

![new64](https://user-images.githubusercontent.com/115363604/224462089-d52ebe00-cc64-4360-b8cd-db74fddb5d2f.png)

![new65](https://user-images.githubusercontent.com/115363604/224462094-8e5e4976-3374-4282-ab2c-792d358bb5b0.png)






  
  
  
  

