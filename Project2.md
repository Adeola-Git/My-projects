
# Devops Project-2

#### WEB STACK IMPLEMENTATION (LEMP STACK)
#### INSTALLING THE NGINX WEB SERVER

* Commands:

       * sudo apt update
  
      * sudo apt install nginx
  
      * sudo systemctl status nginx
 
![test14](https://user-images.githubusercontent.com/115363604/198842839-a9397542-7b36-4e37-9325-163c76aaf8b7.png)

* I will try to access nginx locally on my ubuntu shell:

          * curl http://localhodt:80
    
    ![test15](https://user-images.githubusercontent.com/115363604/198842936-10edddeb-2fad-406c-a170-063f26504070.png)

* I will go to my EC2 instant and enable my inbount rules to allow HTTP from anywhere
* I will go to web browser to access nginx using the DNS compute information on my EC2 instance

![test16](https://user-images.githubusercontent.com/115363604/198843089-fccf1538-255f-47b1-9f51-1bcc6e5320aa.png)

#### STEP 2: INSTALLING MYSQL
Commands: 

      * sudo apt install mysql-server

![test17](https://user-images.githubusercontent.com/115363604/198843577-d8e66b7e-55a6-4d89-9d7a-a5672f04c1cc.png)
      
* Before I run a sucurity script that comes pre-installed with MYSQL, I need to set a password for the root user, using mysql_native_password as a default authentication method.

      * ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
 
 ![test18](https://user-images.githubusercontent.com/115363604/198843666-690bb73d-c38d-4a4c-9131-a400ede8948b.png)
 
 * I will start interactive script by running:

      * sudo mysql_secure_installation

![test19](https://user-images.githubusercontent.com/115363604/198843822-dca09fb2-3186-4884-9aba-010b472388f1.png)

* I will test if I am able to log in to the MYSQL console by typing:

      * sudo mysql -p
 
 ![test20](https://user-images.githubusercontent.com/115363604/198843872-53399c01-834d-4edb-b7da-952366cedcce.png)
 
 #### STEP 3: INSTALLING PHP
 Commands:
 
      * sudo apt install php-fpm php-mysql
  
  ![test21](https://user-images.githubusercontent.com/115363604/198845143-c415f8c7-25d8-4e9a-976a-280f8f5c0f85.png)
  
  #### STEP 4: CONFIGURING NGINX TO USE PHP PROCESSOR
  * Commands:
  
          * sudo mkdir /var/www/projectLEMP ( Create root web directory)
          * sudo chown -R $USER:USER /var/www/projectLEMP (Assign ownwership)
          * sudo nano /etc/nginx/sites-available/projectLEMP (Create new configuration file)
          * sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enable (Activate the configuration)
          * sudo nginx -t (Test configration)
 
 ![test22](https://user-images.githubusercontent.com/115363604/198845432-bd69827c-820d-41d0-ad7e-176775ad63f0.png)

      * sudo unlink /etc/nginx/sites-enable/default (Disable default Nginx host)
      * sudo systemctl reload nginx ( To apply changes)
 
 ![test23](https://user-images.githubusercontent.com/115363604/198845527-854fb5dc-baf4-4109-a07c-57a596f57cc3.png)
 
 * Commands:
      * sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html (Create an index.html file in the location)
      
* I will refresh my web browser

![test24](https://user-images.githubusercontent.com/115363604/198845746-7a13bb1e-dd5d-463b-a297-c03c1d77a02b.png)

#### STEP 5:TESTING PHP WITH NGINX

* Commands:
      * sudo nano /var/www/projectLEMP/info.php (Create a PHP file in document root)
  
  ![test25](https://user-images.githubusercontent.com/115363604/198845951-adb363a7-4135-4468-b9bb-edbab7d8cf62.png)
  
  * I will refresh my bed browser adding "/inf.php" to the URL

  ![test26](https://user-images.githubusercontent.com/115363604/198846046-13cc5fdf-07aa-48f2-8770-c7e7057149b1.png)

#### STEP 6: RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
* Commands:
      * sudo mysql -p (Connect to MYSQL)
      * mysql -u example_user -p (Test new user permission to MYSQL)


      
        
  
 
