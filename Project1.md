
## Devops Project-1
#### WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
##### STEP 1: INSTALLING APACHE AND UPDATING THE FIREWALL
* Commands:
  * sudo apt update
  * sudo apt install apache2
  * sudo systemctl status apache2
  
![test 1](https://user-images.githubusercontent.com/115363604/198828193-1e72d93d-eec5-49a2-95f0-a350a0b1941b.png)

* I will add a rule to EC2 configuration to open inbound connection through port 80
* I will try and check if I can access it locally on my unbuntu shell with the command:
     * curl http://localhost:80 
  
  ![test11](https://user-images.githubusercontent.com/115363604/198838966-31e51b2e-60de-4f3c-9028-f3c8e8ba2fa1.png)

* I will test if my Apache HTTP server can respond to request from the internet. I will open my web browser and try to access using the following url
    *  http://44.202.243.191:80

![test2](https://user-images.githubusercontent.com/115363604/198828831-66975455-92db-4769-903b-bcb3c89d23a2.png)

##### STEP2: INSTALLING MYSQL
* Commands:
     * sudo apt install mysql-server -y

 ![test3](https://user-images.githubusercontent.com/115363604/198839421-73fbf735-319c-49e5-9dc2-8d5c0084d69e.png)

* Before I run a sucurity script that comes pre-installed with MYSQL, I need to set a password for the root user, using mysql_native_password as a default authentication method.
     * ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

![test4](https://user-images.githubusercontent.com/115363604/198839721-9bdc6b21-f38c-4d1e-9400-3a0cf520abef.png)
* I will start interactive script by running:
     * sudo mysql_secure_installation
  
  ![test5](https://user-images.githubusercontent.com/115363604/198839842-ffe5f543-1161-4a3e-b760-d9014f08525e.png)

* i will test if I am able to log in to the MYSQL console by typing:
     * sudo mysql -p
 
 ![test6](https://user-images.githubusercontent.com/115363604/198839926-4a5970d4-6f1f-46ae-99bd-cbdd2505a04d.png)
 
 ##### STEP3: INSTALLING PHP
 * Commands:
 
       * sudo apt install php install php libapache2-mod-php php-mysql
 
   ![test7](https://user-images.githubusercontent.com/115363604/198840118-2943202a-8927-4297-8d3c-8514837e6eff.png)

##### STEP 4: CREATING A VIRTUAL HOST FOR MY WEBSITE USING APACHE
* Commands:
     * sudo mkdir /var/www/projetlamp (Create project directory)
     * sudo chown -R $USER:USER /var/www/projectlamp (Change user ownership)
     * sudo vi /etc/apache2/sites-available/projectlamp.conf (Create Virtual Host conf for project)
     * sudo ls /etc/apache2/sites-available ( Show the new file) 
     * sudo a2ensite projectlamp (Enable project)
     * sudo s2dissite 000-default (Disable default site)
     * sudo apache2ctl configtest (Test configurations)
     * sudo systemctl reload apache2 (Reload Apache2 service)
   
 ![test8](https://user-images.githubusercontent.com/115363604/198840980-bf7cfd36-cd70-451b-9a57-d5b57babdc7a.png)

* Commands:
     * sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html (Create an index.html file in web root location)
     * ls -l /var/www/projectlamp (Check if there is something in the folder)
 ![test9](https://user-images.githubusercontent.com/115363604/198840899-e4873dff-1516-482d-ba78-5eeb54c6758e.png)
 
 * I will go to my web browser and refresh the page
 
 ![test10](https://user-images.githubusercontent.com/115363604/198841238-33f8576f-9ecb-493b-b295-1545c7197926.png)
 
 ##### STEP 5: ENABLE PHP ON THE WEBSITE
 * Commands:
        * sudo vi /etc/apache2/mods-enabled/dir.conf (Edit dir.conf to place index.php at higher precedence)
        * sudo systemctl reload apache2 ( So the changes can take effect)
        * vi /var/www/projectlamp/index.php (Create PHP index page)
    
    ![test12](https://user-images.githubusercontent.com/115363604/198841608-39904829-5d44-4779-9915-1c34cbe97b76.png)

* I will go back to my web broswer and refresh the page.

![test13](https://user-images.githubusercontent.com/115363604/198841666-0cddbb1b-63e8-47fc-9e3b-97e3bc8ed229.png)


 
  
