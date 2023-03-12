
# Devops Project-10

## LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

#### STEP 1: CONFIGURE NGINX AS A LOAD BALANCER
* You can either uninstall Apache from the existing Load Balancer server, or create a fresh installation of Linux for Nginx.
* Uninstalled Apache from the existing Load Balancer server with the command:
  - sudo apt remove apache2

![new105](https://user-images.githubusercontent.com/115363604/224520925-b5c1e6c3-a479-4a79-8089-d947cb52f744.png)

* Open TCP port 80 for HTTP connections, also open TCP port 443 – this port is used for secured HTTPS connections)

![new104](https://user-images.githubusercontent.com/115363604/224520834-497ae4a9-1caf-4c65-b7f8-8df9c7da2c7e.png)

* Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers.
* Update the instance and Install Nginx.
  - sudo apt update
  - sudo apt install nginx
  
![new106](https://user-images.githubusercontent.com/115363604/224520927-d9c95231-5fef-4530-9c72-8c53ae099667.png)

* Open the default nginx configuration file
  - sudo vi /etc/nginx/nginx.conf

![new125](https://user-images.githubusercontent.com/115363604/224523420-7c8515de-41c1-4ea3-bfd2-9b2e5c8623a5.png)

* Restart Nginx and make sure the service is up and running
  - sudo systemctl enable && sudo systemctl start nginx
  - sudo systemctl status nginx

![new107](https://user-images.githubusercontent.com/115363604/224521252-6fdfda71-1d80-4a56-9f00-6e869fb866fd.png)

### REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

* Register domain at Godaddy.com
* Search for the domain you want and register it.

![NEW109](https://user-images.githubusercontent.com/115363604/224521486-7b42cace-ae53-48c1-81e4-db75fdc54463.png)

#### Go to the AWS console
* Seacrh for *route 53 dashboard* and do the following:
  - create hosted host ( Go back to the *godaddy.com* and copy the domain name )
  - domain name ( Paste the domain name into it )
  - public hosted zone
  - create
  
![new110](https://user-images.githubusercontent.com/115363604/224521706-80ce7e88-d4b2-41e5-835d-7401ffbb68a2.png)

* Go back to your doamin and copy each of the *Value/Route traffic to* in the *route 53 dashboard* under records and add it to the nameservers in your doamin. So that   route 53 and   the domain name can be connected to each other.

![new111](https://user-images.githubusercontent.com/115363604/224521869-49166ef4-b42a-4801-893e-481b4d713ee2.png)

* In Route 53, you will create a record, the value will be the load balancer public IP address.
* You will create another record and add the record name as *www*, the value will be the public IP address of the Load balancer then create.

![Screenshot 2023-03-11 221140](https://user-images.githubusercontent.com/115363604/224522112-58802165-6916-4844-871e-0249c198e52b.png)

* Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://<your-domain-name.com>

![new112](https://user-images.githubusercontent.com/115363604/224522234-48c12b41-7b0e-4cca-bc4a-ee6fc5cfd5ad.png)

* It came out with an error, which I was able to fix by doing the following;
  - Spin up my *NFS* server and remounted it. Then restarted and check the status which came out ok.
  - Spin up my *webservers* and remounted the */var/www* directory. Started and check the status and everything came out ok.
  - Went back to reload nginx server and it came out good too.
 
![new113](https://user-images.githubusercontent.com/115363604/224522401-7c915976-89f3-4ca6-93bb-a54fb93a47b0.png)

* Install certbot and request for an SSL/TLS certificate
* Make sure snapd service is active and running
  - sudo systemctl status snapd

![new114](https://user-images.githubusercontent.com/115363604/224522495-e566dc2d-e765-4d3d-951a-73d9144e3484.png)

* Install certbot
  - sudo snap install --classic certbot

![new115](https://user-images.githubusercontent.com/115363604/224522558-de68cdb1-9d5b-4320-b865-c4b1f4ab8a5e.png)

* Go to the *nginx* configuration file and edit it by adding the domain name to it.
* uncomment out include /etc/nginx/sites-enabled/*;.
  - sudo vi /etc/nginx/nginx.conf

![new117](https://user-images.githubusercontent.com/115363604/224522788-54b7a6fc-462f-46dc-816f-48e6731e8780.png)

![new118](https://user-images.githubusercontent.com/115363604/224522791-4184e560-79bb-468d-8bd1-80df9c95c230.png)

* Request your certificate (just follow the certbot instructions )
* You will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file
  - sudo certbot --nginx

![new116](https://user-images.githubusercontent.com/115363604/224522853-5765d9dd-294c-4298-a870-d11fc363c28f.png)

* Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>

* You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string.
* Click on the padlock icon and you can see the details of the certificate issued for your website.

![new119](https://user-images.githubusercontent.com/115363604/224522978-6ed29a89-d208-4bb7-ae1b-3b0eb8d4fda1.png)

![new120](https://user-images.githubusercontent.com/115363604/224522982-7145764c-9825-4474-8a13-d8ae1332e25a.png)

* Set up periodical renewal of your SSL/TLS certificate
* By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

* You can test renewal command in dry-run mode.
  - sudo certbot renew --dry-run

![new121](https://user-images.githubusercontent.com/115363604/224523069-0625476b-74df-40ee-83e7-5e96eb29d2ae.png)

* Best pracice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.
* To do so, lets edit the crontab file with the following command:
  - crontab -e

* Add following line:
  - * */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1

![new122](https://user-images.githubusercontent.com/115363604/224523113-520d85fe-22cd-4b69-aafd-ac693bcfdb56.png)

* Go back and refresh the page.

![new123](https://user-images.githubusercontent.com/115363604/224523196-a44cda1d-1efa-4f9b-b5fc-c52eb24601be.png)

![new124](https://user-images.githubusercontent.com/115363604/224523199-40b52f11-3013-4ba4-826c-990cf5ff4b00.png)




 
 




