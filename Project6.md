
# Devops Project-6

## WEB SOLUTION WITH WORDPRESS

#### STEP 1: PREPARE A WEB SERVER
* Launch an EC2 instance that will serve as *Web Server*. 
* Create 3 volumes in the same AZ as my Web Server EC2, each of 10 GB.

![test116](https://user-images.githubusercontent.com/115363604/200194051-5da0041e-4891-4038-b59e-131cec014aae.png)

* Attach all three volumes one by one to the Web Server EC2 instance

![test117](https://user-images.githubusercontent.com/115363604/200194194-86c3a88f-b6b6-4010-b5f7-fb2d474ec3f3.png)

* Open up the Linux terminal to begin configuration

![test118](https://user-images.githubusercontent.com/115363604/200194290-27f8abd6-03b1-47b3-8757-27d477d1d2e7.png)

* Use *lsblk* command to inspect what block devices are attached to the server. 

![test119](https://user-images.githubusercontent.com/115363604/200194394-d4882030-02b7-43e5-a048-2e93a14382b7.png)

* Use *df -h* command to see all mounts and free space on the server

![test120](https://user-images.githubusercontent.com/115363604/200194462-c7c77098-de0a-41e5-a1cd-f7d367c1718e.png)

* Use *gdisk* utility to create a single partition on each of the 3 disks
  - sudo gdisk /dev/xvdf
  - sudo gdisk /dev/xvdg
  - sudo gdisk /dev/xvdh

![test121](https://user-images.githubusercontent.com/115363604/200194540-2d6390c3-ce80-4a3f-916f-8c31f71ca987.png)

* Use *lsblk* utility to view the newly configured partition on each of the 3 disks

![test122](https://user-images.githubusercontent.com/115363604/200194818-dca75c46-d686-424b-889d-790adc6eaad4.png)

* Install *lvm2* package using the command:
  - sudo yum install lvm2
  - sudo lvmdiskscan ( To check for available partitions )
 
![test123](https://user-images.githubusercontent.com/115363604/200194927-de2d0203-1b5c-42cc-a3d3-4b74c53e2dd3.png)

* Use *pvcreate* utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
  - sudo pvcreate /dev/xvdf1
  - sudo pvcreate /dev/xvdg1
  - sudo pvcreate /dev/xvdh1
 
![test124](https://user-images.githubusercontent.com/115363604/200195089-f1891fe9-f384-4843-8ccc-02ee0c07ce50.png)

 * To verify that the Physical volume has been created successfully run the command:
   - sudo pvs
![test125](https://user-images.githubusercontent.com/115363604/200195113-151e0a79-b390-4f38-a63a-dca64d57440a.png)

* Use *vgcreate* utility to add all 3 PVs to a volume group (VG). Name the VG *webdata-vg*
  - sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
  - sudo vgs ( To verify that the VG has been created successfully )

![test126](https://user-images.githubusercontent.com/115363604/200195278-f12bffb0-51b8-466a-8ef7-b8cf6755f41e.png)

* Use *lvcreate* utility to create 2 logical volumes. *apps-lv* and *logs-lv*
* *apps-lv* will be used to store data for the Website while, *logs-lv* will be used to store data for logs
   - sudo lvcraete -n apps-lv -l 14G webdata-vg
   - sudo lvcreate -n logs-lv -l 14G webdata -vg
   - sudo lvs ( To verify that the Logical Volume has been created successfully )
 
![test127](https://user-images.githubusercontent.com/115363604/200195643-fd018e20-bd9f-4af8-b61e-9ee620b72817.png)

* Verify the entire set up with the command:
  - sudo vgdisplay -v #view complete setup - VG, PV, and LV
  - suod lsblk

![test128](https://user-images.githubusercontent.com/115363604/200195744-b16adc8c-0156-446d-8379-bf70e731693f.png)

* Format the logical volumes with *ext4* filesystem using the command:
  - sudo mkfs -t ext4 /dev/webdata-vg/apps-lv

![test129](https://user-images.githubusercontent.com/115363604/200195889-27b99bc3-e497-4116-80b4-d1251a4d0cbf.png)

* Command:
  - sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

![test130](https://user-images.githubusercontent.com/115363604/200195916-7d2a2de8-287c-41f7-95cf-6f1daefc69b3.png)

* Commands:
  - sudo mkdir -p /var/www/html ( Directory to store website files )
  - sudo mkdir -p /home/recovery/logs ( Directory to store backup of log data )
  - sudo mount /dev/webdata-vg/apps-lv /var/www/html/ ( To mount */var/www/html* on *apps-lv* logical volume )
  - sudo rsync -av /var/log/. /home/recovery/logs/ ( To back all the files in the log directory which is regired before mounting the file sys tem )
  
![test131](https://user-images.githubusercontent.com/115363604/200196239-a90a31c4-799e-4032-8f3b-91f4d492c723.png)

* Commands:
  - sudo mount /dev/webdata-vg/logs-lv /var/log
  - sudo rsync -av /home/recovery/logs/. /var/log
 
![test132](https://user-images.githubusercontent.com/115363604/200196719-a2007ce9-5650-46d3-94d9-7812468e7e18.png)

#### UPDATE THE '/ETC/FSTAB' FILE
* Use the UUID of the device to update the */etc/fstab* file with command:
  - sudo blkid ( To get my UUID number )
  - sudo vi /etc/fstab

![test133](https://user-images.githubusercontent.com/115363604/200199213-1471112d-56f8-4be2-8f9a-090e7e31d9ee.png)

* Test the configuration and reload the daemon with the command:
  - sudo mount -a
  - sudo systemctl daemon-reload
  
![test134](https://user-images.githubusercontent.com/115363604/200199723-b1ad2d53-7822-4676-b8c5-1091ac81ee1d.png)

* Verify the setup by running *df -h* 

![test135](https://user-images.githubusercontent.com/115363604/200199753-9d2d36be-e15e-42da-9a45-dc1729ab1696.png)

#### STEP 2: PREPARE THE DATABASE SERVER
* Launch a second RedHat EC2 instance that will have a role *DB Server*.
* Create 3 volumes in the same AZ as my Web Server EC2, each of 10 GB.

![test136](https://user-images.githubusercontent.com/115363604/200199922-90738a24-89ab-44fc-8e79-2d75e8bb3725.png)

* Attach all three volumes one by one to the Web Server EC2 instance

![test137](https://user-images.githubusercontent.com/115363604/200199937-8e60db7a-1f81-40ae-a3d6-dc6feb58468e.png)


 * Open up the Linux terminal to begin configuration

![test138](https://user-images.githubusercontent.com/115363604/200200020-dde5424e-05d1-461b-b8e6-89252565bf0f.png)

* Use *lsblk* command to inspect what block devices are attached to the server. 

![test139](https://user-images.githubusercontent.com/115363604/200200098-0ef8382d-d067-47d1-a3d6-1167f639903e.png)

* Use *gdisk* utility to create a single partition on each of the 3 disks
  - sudo gdisk /dev/xvdf
  - sudo gdisk /dev/xvdg
  - sudo gdisk /dev/xvdh

![test140](https://user-images.githubusercontent.com/115363604/200200247-48579c16-a268-4f20-9ca9-2b690e63252d.png)

* Use *lsblk* utility to view the newly configured partition on each of the 3 disks

![test141](https://user-images.githubusercontent.com/115363604/200200342-9ed33d4e-5958-4d51-a6e2-f9fbee3b6389.png)

* Install *lvm2* package using the command:
  - sudo yum install lvm2
  - sudo lvmdiskscan ( To check for available partitions )
 
![test142](https://user-images.githubusercontent.com/115363604/200200400-42f7139f-6c1c-4ecd-b07c-c150bc38da1c.png)

* Use *pvcreate* utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
  - sudo pvcreate /dev/xvdf1
  - sudo pvcreate /dev/xvdg1
  - sudo pvcreate /dev/xvdh1

![test143](https://user-images.githubusercontent.com/115363604/200200448-0a2f86e0-48f5-445c-836d-1afae2a39bcc.png)

* To verify that the Physical volume has been created successfully run the command:
   - sudo pvs
   
![test144](https://user-images.githubusercontent.com/115363604/200200534-7e0d36bd-66a3-45e3-a6fd-6e05f988b087.png)

* Use *vgcreate* utility to add all 3 PVs to a volume group (VG). Name the VG *vg-database*
  - sudo vgcreate vg-database /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
  - sudo vgs ( To verify that the VG has been created successfully )

![test145](https://user-images.githubusercontent.com/115363604/200200644-3a5520d1-dc08-4b46-8548-4a09386b9580.png)

* Use *lvcreate* utility to create the logical volumes, *db-lv*
   - sudo lvcraete -n db-lv -l 20G vg-database
   - sudo lvs ( To verify that the Logical Volume has been created successfully )

![test146](https://user-images.githubusercontent.com/115363604/200200864-9e224491-cb4d-4021-8ee0-47c691e4961d.png)

* Verify the entire set up with the command:
  - sudo vgdisplay -v #view complete setup - VG, PV, and LV
  - sudo lsblk

* Format the logical volumes with *ext4* filesystem using the command:
  - sudo mkfs.ext4 /dev/vg-database/db-lv

![test147](https://user-images.githubusercontent.com/115363604/200201072-d7ba5595-b5f2-4309-9451-e63b86fb5a84.png)

* Make a directory to use as a mount point for the formatted disks.
* Commands:
  - sudo mkdir /db
  - sudo mount /dev/vg-database/db-lv /db ( To mount */db* on *db-lv* logical volume )
  - df -h ( To check the mounted point )

![test148](https://user-images.githubusercontent.com/115363604/200201303-e6bbb7b1-2a2f-43c7-82ed-72e3e7c58e58.png)

#### UPDATE THE '/ETC/FSTAB' FILE
* Use the UUID of the device to update the */etc/fstab* file with command:
  - sudo blkid ( To get my UUID number )
  - sudo vi /etc/fstab

![test149](https://user-images.githubusercontent.com/115363604/200201419-3ae9f80c-bf4a-4367-9d97-363f51d9808f.png)

* Test the configuration and reload the daemon with the command:
  - sudo mount -a
  - sudo systemctl daemon-reload
  
![test150](https://user-images.githubusercontent.com/115363604/200201472-4b5c13c8-dedd-4d7d-8e5f-31303694974c.png)

* Verify the setup by running *df -h* 

![test151](https://user-images.githubusercontent.com/115363604/200201538-1772e2a0-eedc-4921-a138-05690f5fd63a.png)

####  STEP 3: INSTALL WORDPRESS ON YOUR WEB SERVER EC2
* Commands:
  - sudo yum -y update ( Update the repository )
  - sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
  
![test152](https://user-images.githubusercontent.com/115363604/200201721-0103d6ae-db16-49c3-9342-8c12bda1de62.png)

* Start Apache with the command:
  - sudo systemctl enable httpd
  - sudo systemctl start httpd
 
![test153](https://user-images.githubusercontent.com/115363604/200201761-84bb3363-d436-4046-91a8-724e9df5f9e8.png)

* Install PHP and its dependencies with the commands:
  - sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  - sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
  - sudo yum module list php
  - sudo yum module reset php
  - sudo yum module enable php:remi-7.4
  - sudo yum install php php-opcache php-gd php-curl php-mysqlnd
  - sudo systemctl start php-fpm
  - sudo systemctl enable php-fpm
  - setsebool -P httpd_execmem 1 ( To instruct SELinux to allow Apache to excute the PHP code via PHP-FPM)

![test154](https://user-images.githubusercontent.com/115363604/200201971-fa54188a-450a-4a7b-a1ac-762cb2757009.png)

* Restart Apache
  - sudo systemctl restart httpd
  
![test155](https://user-images.githubusercontent.com/115363604/200202042-53de2e13-d9f7-489a-be6e-daf49dde4c30.png)

* Download wordpress and copy wordpress to */var/www/html*
* Commands:
  - mkdir wordpress
  - cd wordpress
  - sudo wget http://wordpress.org/latest.tar.gz
  - sudo tar xzvf latest.tar.gz
  - sudo wordpress
  - sudo cp -R wp-config-sample.php wp-config.php
  - cat wp-conf.php
  - cd wordpress
  - sudo cp -R wordpress/. /var/www/html/ ( Copy *wordpress* file inot */var/www/html* )
  - sudo ls -l /var/www/html ( List the content )
 
![test156](https://user-images.githubusercontent.com/115363604/200202532-850faf0d-6575-475b-9f79-5586d0b892b2.png)

* Configure SELinux Policies with the commands:
  - sudo chown -R apache:apache /var/www/html/wordpress
  - sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
  - sudo setsebool -P httpd_can_network_connect=1
  - sudo setsebool -p httpd_can_network_connect_db=1
  
![test157](https://user-images.githubusercontent.com/115363604/200202708-6b348c7a-093a-4588-a60f-c41326d6806a.png)

#### STEP 4: INSTALL MYSQL ON YOUR DB SERVER EC2
* Commands:
  - sudo yum update
  - sudo yum install mysql-server

![test158](https://user-images.githubusercontent.com/115363604/200202816-87c0ed32-b23c-469e-a021-2b662db73d89.png)

* Verify that the service is up and running 
  - sudo systemctl status mysqld
  
![test159](https://user-images.githubusercontent.com/115363604/200202977-4fb0a43e-e3de-4e6d-848d-11c548c3948a.png)
  
* It is not running so restart the service and enable it so it will be running even after reboot
  - sudo systemctl restart mysqld
  - sudo systemctl enable mysqld
  - sudo systemctl status mysqld

![test160](https://user-images.githubusercontent.com/115363604/200203019-9032e197-d90f-424a-a61d-a96587164c2a.png)

* Edit */etc/my.cnf* to add the web server IP address
  - sudo vi /etc/my.cnf

![test161](https://user-images.githubusercontent.com/115363604/200203297-9a5bcce5-a34b-49e0-9e65-2d29d5bd7bc2.png)


#### STEP 5: CONFIGURE DB TO WORK WITH WORDPRESS
* Commands:
  - sudo mysql_secure_installation
  - sudo mysql -u root -p
  - CREATE DATABASE wordpress;
  - CREATE USER `myuser`@`172.31.80.29` IDENTIFIED BY 'T';
  - GRANT ALL ON wordpress.* TO 'myuser'@'172.31.80.29';
  - FLUSH PRIVILEGES;
  - SHOW DATABASES;
  - exit

![test162](https://user-images.githubusercontent.com/115363604/200203502-7a527a85-be5a-4331-8454-dec0a0fb976d.png)

#### STEP 6: CONFIGURE WORDPRESS TO CONNECT TO REMOTE DATABASE
* Open MySQL port 3306 on DB server EC2 allowing access to the DB server only from the Web Server's IP address

![test163](https://user-images.githubusercontent.com/115363604/200203649-f25f08e2-3a00-404e-bf7d-bde7bcf2f5b9.png)

* Install MySQL client and test to ensure it can connect from the Web Server to the DB server by using *mysql-client*
 * Commands:
  - sudo yum install mysql-server
  - sudo systemcetl start mysqld
  - sudo systemctl enable mysqld
  - sudo systemctl status mysqld ( Verify if MySLQ is working 0
  
![test164](https://user-images.githubusercontent.com/115363604/200203936-734a711b-39b0-4832-a346-777730515c14.png)
  
* Test connection from *web server* to *db server* using the command:
  - sudo mysql -h 172.31.86.110 -u myuser -p
  - show database; ( Verify if *show databses* can be successfully executed )

![test165](https://user-images.githubusercontent.com/115363604/200204236-6dee95c0-b3b0-4ecc-84b9-ed876bc952f6.png)

* Enable TCP port 80 in Inbound Rules configuration for Web Server EC2 to allow conection from anywhere

![test166](https://user-images.githubusercontent.com/115363604/200204410-4bab746f-735a-40c7-8df0-90031eb462e9.png)

* Go to the *wp-config.php* directory to change the database name, username, password and the IP address with the command:
  - sudo vi wp-conf.php

![test167](https://user-images.githubusercontent.com/115363604/200204591-b2cc7e45-737d-4407-9959-93a388cebe8e.png)

* Change permission and configuration so Apache could use WordPress with the commands:
  - sudo systemctl restart httpd
  - sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup ( Disable the default page of Apache and move it to another directory)
  - ls -l /etc/httpd/conf.d/welcome.conf_backup
  
![test168](https://user-images.githubusercontent.com/115363604/200204947-d892a012-7150-4646-aba9-dcc73d798a4f.png)

* Access from the web browser the link to the WordPress
  - http://54.226.175.24/wordpress/
  
![test169](https://user-images.githubusercontent.com/115363604/200205123-5ea1c1d3-10d3-45cf-afd7-67d0c11c626f.png)

* Put the DB credentials:

![test170](https://user-images.githubusercontent.com/115363604/200205350-381489fb-bc4c-4e25-a972-c535d5817fcd.png)

![test171](https://user-images.githubusercontent.com/115363604/200205367-26f3216f-6050-488e-bf66-4c994f476184.png)

![test172](https://user-images.githubusercontent.com/115363604/200205409-16961ef0-3684-44ca-90ae-a463003e7201.png)








