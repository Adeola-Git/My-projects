
## Devops Project-1
#### WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
##### STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL
* Commands:
  * sudo apt update
  * sudo apt install apache2
  * sudo systemctl status apache2
  
![test 1](https://user-images.githubusercontent.com/115363604/198828193-1e72d93d-eec5-49a2-95f0-a350a0b1941b.png)

* I will add a rule to EC2 configuration to open inbound connection through port 80
* I will try check if I can access it locally on my unbuntu shell with the command:
     * curl http://localhost:80 
* I will test if my Apache HTTP server can respond to request from the internet. I will open my web browser and try to access using the following url
    *  http://44.202.243.191:80

![test2](https://user-images.githubusercontent.com/115363604/198828831-66975455-92db-4769-903b-bcb3c89d23a2.png)
