
# Devops Project-5

## CLIENT-SERVER ARCHITECTURE WITH MYSQL

#### TASK
* Implement a Client Server Architecture using MySQL Database Management System (DBMS)
* Create and configure two Linux-based virtual server (EC2 instances in AWS) named *mysql server* and *mysql client*
* I will run the commands:
  - sudo apt update ( I will update both servers being a new server )
  - sudo apt upgrade ( I will upgrade both servers )
* On *mysql server* Linux Server I will install MySQL Server software
  - sudo apt install mysql-server -y

![test105](https://user-images.githubusercontent.com/115363604/200191676-9cc5575c-6ea0-4d0d-903b-639466e5c260.png)

* I will enable *mysql server software* on my *mysql server* with the command:
  - sudo systemctl enable mysql

![test106](https://user-images.githubusercontent.com/115363604/200191710-ba450484-0f35-463a-9a7e-b0c19fcb19a8.png)

* On *mysql client* Linux Server I will install MySQL Client software
  - sudo apt install mysql-client -y

![test107](https://user-images.githubusercontent.com/115363604/200191778-76d9c786-0f3a-4003-bb63-0b6c8cb561a0.png)

* By default, both my EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses.
* Use *mysql server's* local IP address to connect from *mysql client*. 
* MySQL server uses TCP port 3306 by default, so I will have to open it by creating a new entry in *inbound rules* in *mysql server* Security Groups.
* For extra security, I will not allow all IP addresses to reach my *mysql server* and allow only the specified local IP address of my *mysql client*

![test108](https://user-images.githubusercontent.com/115363604/200192229-1e1596ed-3fca-4993-bcf2-1ee8ce8c7fe8.png)

* For my *mysql server* I will create mysql user and database
  - sudo mysql_secure_installation

![test109](https://user-images.githubusercontent.com/115363604/200192292-406d5616-1a64-4242-929e-b2dd9540bfcf.png)

* The command gave an error asking me to use a specific command, so I have to use the command:
  - sudo mysql
  - ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

![test111](https://user-images.githubusercontent.com/115363604/200192506-2a733bb7-1c3b-4cd1-8a5e-27d2e24c2b49.png)

* I will run the command again and follow the prompt. I did not change the root password
  - sudo_mysql_secure_installation

![test112](https://user-images.githubusercontent.com/115363604/200192644-b68ec8f8-e5a6-4a96-ba7d-cc05f04edf31.png)

* I will create my database user with the command:
  - sudo mysql -u root -p
 
![test113](https://user-images.githubusercontent.com/115363604/200192735-f3b6c2f8-9917-46a9-b523-5256d5761c43.png)

* I will need to configure *MySQL server* to allow connections from remote hosts to replace *127.0.0.1* to *0.0.0.0* with the command:
  - sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

![test114](https://user-images.githubusercontent.com/115363604/200192901-f8892d45-f0cf-4018-b059-ddef82772256.png)
 
* I will restart my *mysql server* after editing the configuration file, which is the best practice with the command:
  - sudo systemctl restart mysql
 
* From *mysql client* Linux Server I will connect remotely to *mysql server* Database Engine without using *ssh*.
* I will use the *mysql* utility to perform this action and check that I have successfully connected to a remote *MySQL server* and can perform *SQL* queries with the command:
  - sudo mysql -u remote_user -h 172.31.94.127 -p
 
![test115](https://user-images.githubusercontent.com/115363604/200193119-3e9b490b-baa4-4427-a15e-74a1fe4fdc85.png)
 
