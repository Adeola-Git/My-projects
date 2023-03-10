
# Devops Project-7

## DEVOPS TOOLING WEBSITE SOLUTION

#### STEP 1: PREPARE NFS SERVER
* Spin up 4 new EC2 instance with RHEL Linux 8 Operating System. The will be named NFS, websever1, webserver2, webserver3 and webserver4. Ensuring that all 4 instance are inside the same *Subnet*.
* Spin up 1 new EC2 instance with ubuntu 20.04 Operating System. 

![new2](https://user-images.githubusercontent.com/115363604/224214180-7aae87c7-0e73-44fe-87bb-dcc706ca2502.png)

* Create 3 volumes in the same AZ as my NFS, each of 10 GB.
* Attach all three volumes one by one to the Web Server EC2 instance

![new3](https://user-images.githubusercontent.com/115363604/224214381-4c5e662e-6893-4154-98fa-bd897af782c1.png)

* Open up the Linux terminal to begin configuration of my NFS server.

![new4](https://user-images.githubusercontent.com/115363604/224214756-100c1652-cde8-442d-8ff4-95d6c9d868a5.png)

* Use *lsblk* command to inspect what block devices are attached to the server.
* Use *df -h* command to see all mounts and free space on the server.
* * Use *gdisk* utility to create a single partition on each of the 3 disks
  - sudo gdisk /dev/xvdf
  - sudo gdisk /dev/xvdg
  - sudo gdisk /dev/xvdh

* Use *lsblk* utility to view the newly configured partition on each of the 3 disks

![new5](https://user-images.githubusercontent.com/115363604/224215340-20108878-3124-490d-9a15-942c2339cad5.png)

* Install *lvm2* package using the command:
  - Sudo yum update -y ( To update all packages to their recent version )
  - sudo yum install lvm2
  - sudo lvmdiskscan ( To check for available partitions )
  - Lsblk ( To check the attached partition table )
![Screenshot 2023-03-09 222717](https://user-images.githubusercontent.com/115363604/224215871-2bf275c2-dde3-4e7d-bb7e-d39e2c113484.png)
* Use *pvcreate* utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
  - sudo pvcreate /dev/xvdf1
  - sudo pvcreate /dev/xvdg1
  - sudo pvcreate /dev/xvdh1

![new6](https://user-images.githubusercontent.com/115363604/224216069-d2b2a614-8b60-4410-b7e7-2287e56e507f.png)

* To verify that the Physical volume has been created successfully run the command:
   - sudo pvs
* Use *vgcreate* utility to add all 3 PVs to a volume group (VG). Name the VG *webdata-vg*
  - sudo vgcreate webdata-vg /dev/xvdf1 /dev/xvdg1 /dev/xvdh1
  - sudo vgs ( To verify that the VG has been created successfully )
 
 ![new7](https://user-images.githubusercontent.com/115363604/224216568-606c252e-596b-406a-9ed9-a21aac515133.png)

* Use *lvcreate* utility to create 3 logical volumes. *lv-apps*, *lv-logs and *lv-opt*
* *apps-lv* will be used to store data for the Website while, *logs-lv* will be used to store data for logs
   - sudo lvcreate -n lv-apps -L 9G webdata-vg
   - sudo lvcreate -n lv-logs -L 9G webdata-vg
   - sudo lvcreate -n lv-opt -L 9G webdata-vg
   - sudo lvs ( To verify that the Logical Volume has been created successfully )

![new8](https://user-images.githubusercontent.com/115363604/224217539-1342312f-63f4-4d1e-835b-f605c3cad5d6.png)

* Verify the entire set up with the command:
  - sudo vgdisplay -v #view complete setup - VG, PV, and LV
  - sudo lsblk
 
 ![new9](https://user-images.githubusercontent.com/115363604/224217556-563ad502-4cb9-425f-b714-6289d05fa2a1.png)

* Format the logical volumes with *xfs* filesystem using the command:
  - sudo mkfs -t xfs /dev/webdata-vg/lv-apps
  - sudo mkfs -t xfs /dev/webdata-vg/lv-logs
  - sudo mkfs -t xfs /dev/webdata-vg/lv-opt

![new10](https://user-images.githubusercontent.com/115363604/224218221-805c2d40-a17f-4a86-bf42-a8551e9315aa.png)

* Use the following commands to make directory for the logical volumes created.
  - sudo mkdir /mnt/apps
  - sudo mkdir /mnt/logs
  - sudo mkdir /mnt/opt

* Create mount points on /mnt directory for the logical volumes as follow:
  - Mount lv-apps on /mnt/apps – To be used by webservers
  - Mount lv-logs on /mnt/logs – To be used by webserver logs
  - Mount lv-opt on /mnt/opt – To be used by Jenkins server 

* Use the command belowe to mount the logical volumes.
  - sudo mount /dev/webdata-vg/lv-apps /mnt/apps
  - sudo mount /dev/webdata-vg/lv-logs /mnt/logs
  - sudo mount /dev/webdata-vg/lv-opt /mnt/opt

![new12](https://user-images.githubusercontent.com/115363604/224221571-57d1e169-c085-4346-a7a8-af40624578d7.png)

* Install NFS server, configure it to start on reboot and make sure it is u and running
  - sudo yum -y update
  - sudo yum install nfs-utils -y
  - sudo systemctl start nfs-server.service
  - sudo systemctl enable nfs-server.service
  - sudo systemctl status nfs-server.service

![Screenshot 2023-03-09 231547](https://user-images.githubusercontent.com/115363604/224221982-22d3537a-810e-444f-9f16-8c5b97c2c04d.png)


* Export the mounts for webservers’ subnet cidr to connect as clients. For simplicity, you will install your all three Web Servers inside the same subnet, but in production set up you would probably want to separate each tier inside its own subnet for higher level of security.
To check your subnet cidr – open your EC2 details in AWS web console and locate ‘Networking’ tab and open a Subnet link:


* Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

  - sudo chown -R nobody: /mnt/apps ( To change ownership )
  - sudo chown -R nobody: /mnt/logs
  - sudo chown -R nobody: /mnt/opt

  - sudo chmod -R 777 /mnt/apps ( To change permission )
  - sudo chmod -R 777 /mnt/logs
  - sudo chmod -R 777 /mnt/opt

  - sudo systemctl restart nfs-server.service ( To restart the service )
  - sudo systemctl status nfs-server.service ( To check the status )

![Screenshot 2023-03-09 232408](https://user-images.githubusercontent.com/115363604/224223004-ee895747-1688-49d0-aea3-aa98670cbc2f.png)

* Configure access to NFS for clients within the same subnet. Use the command
  - sudo vi /etc/exports ( Paste the code into it )

 ![new13](https://user-images.githubusercontent.com/115363604/224223606-ed3ae0e5-0af6-48a5-824e-eceac2aff040.png)
 
  - sudo exportfs -arv ( For the webserver to be able to see the mount point and connect to it )

![Screenshot 2023-03-10 005007](https://user-images.githubusercontent.com/115363604/224234403-c7104aa8-c74e-4837-82bf-947715bf8490.png)

![new14](https://user-images.githubusercontent.com/115363604/224223618-68f5ec0e-288c-4eee-abbb-49a66dfbd902.png)

* Check which port is used by NFS and open it using Security Groups (add new Inbound Rule) using this command:
  - rpcinfo -p | grep nfs

![new15](https://user-images.githubusercontent.com/115363604/224224518-e1aa23c1-4605-4be2-b679-e2910b6d3e95.png)

* In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049. 

![new17](https://user-images.githubusercontent.com/115363604/224224312-64217441-d1f8-49e9-bc94-2617b416da38.png)


#### STEP 2 — CONFIGURE THE DATABASE SERVER
* Spin up the *db* instance and run the command:
  - sudo apt update ( Being a freshly deployed server )
  - sudo apt install mysql-server -y
 
 ![new19](https://user-images.githubusercontent.com/115363604/224228155-9a4586d8-02de-4d2a-a4ad-b3263ddfcca3.png)
 
* Connect to mysql and create tooling db
  - sudo mysql
  - CREATE DATABASE tooling;
  - CREATE USER 'webaccess'@'webserver-CIDR' IDENTIFIED BY 'admin';
  - GRANT ALL PRIVILEGES ON tooling.* TO 'webaccess'@'webserver-CIDR' WITH GRANT OPTION;
  - FLUSH PRIVILEGES;
 
 ![new18](https://user-images.githubusercontent.com/115363604/224227185-3c098fde-2603-44ce-90ff-b432ce03c38f.png)
 
 #### Step 3 — Prepare the Web Servers
 
 #### Webserver 1
 
* Launch a new RHEL 8 instance
* Install nfs client with the command:
  - sudo yum install nfs-utils nfs4-acl-tools -y

![new20](https://user-images.githubusercontent.com/115363604/224229104-06278e42-6ce1-4514-bd34-8ee8e294d429.png)
 
 
* Mount /var/www/ and target the NFS server’s export for apps
- sudo mkdir /var/www
- sudo mount -t nfs -o rw,nosuid NFS-Server-Private-IP-Address:/mnt/apps /var/www)

![new21](https://user-images.githubusercontent.com/115363604/224229571-cbe3c120-d9de-4a03-8e71-3116663c6ada.png)

* Use the command *df -h* to view the mounted disk

![new22](https://user-images.githubusercontent.com/115363604/224229786-68edab6a-5ca6-4790-8d0a-eee6b6a00dd6.png)

* Edit /etc/fstab to ensure changes persist after reboot ( sudo vi /etc/fstab )

![new23](https://user-images.githubusercontent.com/115363604/224230854-8c455071-4d61-4bfd-bb91-aff95b519732.png)

* Install Remi’s repository, Apache and PHP with the commands:
  -sudo yum install httpd -y
  - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  - sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
  - sudo dnf module reset php
  - sudo dnf module enable php:remi-7.4
  - sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
  - sudo systemctl start php-fpm
  - sudo systemctl enable php-fpm
  - setsebool -P httpd_execmem 1
 
![new24](https://user-images.githubusercontent.com/115363604/224232198-1b64ad4b-4218-4857-a176-54c6be80e5bb.png)

##### Webserver 2

* Launch a new RHEL 8 instance
* Install nfs client with the command:
  - sudo yum install nfs-utils nfs4-acl-tools -y

![new25](https://user-images.githubusercontent.com/115363604/224232545-b8fec7d6-13fa-4b6f-aa65-263df1a28f08.png)

* Mount /var/www/ and target the NFS server’s export for apps
- sudo mkdir /var/www
- sudo mount -t nfs -o rw,nosuid NFS-Server-Private-IP-Address:/mnt/apps /var/www)

* Edit /etc/fstab to ensure changes persist after reboot ( sudo vi /etc/fstab )

![new26](https://user-images.githubusercontent.com/115363604/224232917-109398b2-fec7-4466-8c41-11367b4af49c.png)

* Install Remi’s repository, Apache and PHP with the commands:
  -sudo yum install httpd -y
  - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  - sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
  - sudo dnf module reset php
  - sudo dnf module enable php:remi-7.4
  - sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
  - sudo systemctl start php-fpm
  - sudo systemctl enable php-fpm
  - setsebool -P httpd_execmem 1
  - df -h ( To view mounted disk )

![new28](https://user-images.githubusercontent.com/115363604/224233537-0e564308-021e-4cbc-b3bb-35f71272973e.png)

##### Webserver 3

* Launch a new RHEL 8 instance
* Install nfs client with the command:
  - sudo yum install nfs-utils nfs4-acl-tools -y

![Screenshot 2023-03-10 004557](https://user-images.githubusercontent.com/115363604/224233685-6bb6b735-a05d-451f-bf4f-e5265cbfd03f.png)


* Mount /var/www/ and target the NFS server’s export for apps
- sudo mkdir /var/www
- sudo mount -t nfs -o rw,nosuid NFS-Server-Private-IP-Address:/mnt/apps /var/www)

![new29](https://user-images.githubusercontent.com/115363604/224234845-4196647f-246a-4f1f-8965-1cade14d5fbf.png)

* Edit /etc/fstab to ensure changes persist after reboot ( sudo vi /etc/fstab )

* Install Remi’s repository, Apache and PHP with the commands:
  -sudo yum install httpd -y
  - sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
  - sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
  - sudo dnf module reset php
  - sudo dnf module enable php:remi-7.4
  - sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
  - sudo systemctl start php-fpm
  - sudo systemctl enable php-fpm
  - setsebool -P httpd_execmem 1
  - df -h ( To view mounted disk )
![new30](https://user-images.githubusercontent.com/115363604/224235435-aa70c01a-68fa-4267-a3b8-1c9ecb8d2e47.png)

##### Back to webserver 1

* Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps.

![new31](https://user-images.githubusercontent.com/115363604/224235997-871cc3e4-29e2-43c9-946f-304702cdca3e.png)

![new32](https://user-images.githubusercontent.com/115363604/224236007-e6c34ae4-e316-4945-a80a-7c946f17d8a0.png)

* Locate the log folder for Apache on the Web Server and mount it to NFS server’s export for logs. Repeat step №4 to make sure the mount point will persist after reboot.
  - sudo /var/log/httpd ( Apache log file )
  - sudo mount -t nfs -o rw,nosuid <nfs ip-address>:/mnt/apps /var/log/httpd
![new33](https://user-images.githubusercontent.com/115363604/224236814-cac53270-fc8f-41af-935a-971a95d92c4e.png)
  
  - sudo vi /etc/fstab

![new34](https://user-images.githubusercontent.com/115363604/224236837-148d0fcd-28eb-43f2-8442-3fd15e516648.png)

* Fork the tooling source code from Darey.io Github Account to your Github account.
  
![new35](https://user-images.githubusercontent.com/115363604/224238076-632d2d55-c03f-46db-aa2d-f45991e3d8d1.png)
  
  - sudo yum install git
  - git init
  - git clone and paste the code copied from *Darey.io Github Account* 
  
![new36](https://user-images.githubusercontent.com/115363604/224238298-ef54fc85-de3f-4fdb-b186-b04cbdb4c24a.png)

  - ls
  - cd tooling/
  - ls
  
 * Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html
  - ls /var/www
  - sudo cp -R html/. /var/www/html ( To move the html file )
  - ls /var/www/html/ls html ( The files will show the samething )
  
![new38](https://user-images.githubusercontent.com/115363604/224238897-f397eb9c-8da7-4217-a733-3386be92b366.png)
  
* Do not forget to open TCP port 80 on the Web Server.
  
![new39](https://user-images.githubusercontent.com/115363604/224239433-3990bb44-45a2-4b7d-99ae-02cdc6add3c6.png)

* Go to the browser and tyoe in the webserver 1 ip address. It is showing an error so I will do the following:
  - sudo systemctl stautus httpd

![Screenshot 2023-03-10 012738](https://user-images.githubusercontent.com/115363604/224240218-aab87a96-ad71-4bf5-98be-92a855b2f2a4.png)

* The webserver is not up and running. Run the commands below to fix it:
    - sudo senteforce 0
    - sudo vi /etc.sysconfig/selinux ( Change it to disabled )
    - sudo systemctl start httpd
    - sudo systemctl status httpd
 
 ![new41](https://user-images.githubusercontent.com/115363604/224240784-43a10412-f7a1-452e-bb7b-16937e2bfd63.png)

 * Refresh the page
 
![new42](https://user-images.githubusercontent.com/115363604/224241028-41822bac-03d8-4beb-be56-1de3e36d6816.png)

* Update the website’s configuration to connect to the database
    - sudo vi /var/www/html/functions.php
    - cd tooling/
    - mysql -h <db ip-address> -u webaccess -p tooling < tooling-db.sql
 
  [new43](https://user-images.githubusercontent.com/115363604/224242042-7b505220-eb35-48f5-929b-367b2c4ca347.png)
                                                                       
 * It came out with an error *command not found* so will install the mysql ( sudo yum install mysql -y ) 
 
##### DB sever
* Edit the configuration file so that *db* server can be accessible by the webservers                                  - sudo vi /etc/mysql/mysql.conf/mysqld.conf ( edit the configuration file )                                                                       
![new44](https://user-images.githubusercontent.com/115363604/224242886-20e8c87f-978f-4f50-8d29-15608e408b8c.png)
  - sudo restart mysql
  - sudo stats mysql                                                                    
![new45](https://user-images.githubusercontent.com/115363604/224243469-2799b5c8-0195-4de6-a8f6-c76e9ddd5b92.png)

##### Webserver 1
* Run the command to test if configuration                                                                        
![new46](https://user-images.githubusercontent.com/115363604/224243641-1f8e18ca-d3d0-4607-9e39-535e8ab04401.png)

##### DB server
* Run the commands below to be sure everything looks good
  - sudo mysql
  - show databases;  
  - use tooling;
  - show tables;
  - select * from users;

 ![new47](https://user-images.githubusercontent.com/115363604/224244740-06e96c03-cb27-4695-8df5-d7d03ede1269.png)

##### Webserver 1
*                                                                        
