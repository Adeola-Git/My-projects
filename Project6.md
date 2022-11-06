
# Devops Project-6

## WEB SOLUTION WITH WORDPRESS

#### STEP 1: PREPARE A WEB SERVER
* I will launch an EC2 instance that will serve as *Web Server*. 
* Create 3 volumes in the same AZ as my Web Server EC2, each of 10 GB.

![test116](https://user-images.githubusercontent.com/115363604/200194051-5da0041e-4891-4038-b59e-131cec014aae.png)

* Attach all three volumes one by one to my Web Server EC2 instance

![test117](https://user-images.githubusercontent.com/115363604/200194194-86c3a88f-b6b6-4010-b5f7-fb2d474ec3f3.png)

* I will open up the Linux terminal to begin configuration

![test118](https://user-images.githubusercontent.com/115363604/200194290-27f8abd6-03b1-47b3-8757-27d477d1d2e7.png)

* I will *lsblk* command to inspect what block devices are attached to the server. 

![test119](https://user-images.githubusercontent.com/115363604/200194394-d4882030-02b7-43e5-a048-2e93a14382b7.png)

* I will use *df -h* command to see all mounts and free space on my server

![test120](https://user-images.githubusercontent.com/115363604/200194462-c7c77098-de0a-41e5-a1cd-f7d367c1718e.png)

* Use *gdisk* utility to create a single partition on each of the 3 disks
  - sudo gdisk /dev/xvdf
  - sudo gdisk /dev/xvdg
  - sudo gdisk /dev/xvdh

![test121](https://user-images.githubusercontent.com/115363604/200194540-2d6390c3-ce80-4a3f-916f-8c31f71ca987.png)

* Use *lsblk* utility to view the newly configured partition on each of the 3 disks

![test122](https://user-images.githubusercontent.com/115363604/200194818-dca75c46-d686-424b-889d-790adc6eaad4.png)

* I will install *lvm2* package using the command:
  - sudo yum install lvm2
  - sudo lvmdiskscan ( To check for available partitions )
 
![test123](https://user-images.githubusercontent.com/115363604/200194927-de2d0203-1b5c-42cc-a3d3-4b74c53e2dd3.png)

* I will use *pvcreate* utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM
  - sudo pvcreate /dev/xvdf1
  - sudo pvcreate /dev/xvdg1
  - sudo pvcreate /dev/xvdh1
 
![test124](https://user-images.githubusercontent.com/115363604/200195089-f1891fe9-f384-4843-8ccc-02ee0c07ce50.png)

 * To verify that my Physical volume has been created successfully i will run the command:
   - sudo pvs
![test125](https://user-images.githubusercontent.com/115363604/200195113-151e0a79-b390-4f38-a63a-dca64d57440a.png)

* I will use *vgcreate* utility to add all 3 PVs to a volume group (VG). I will name the VG *webdata-vg*
  - sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1
  - sudo vgs ( To verify that my VG has been created successfully )

![test126](https://user-images.githubusercontent.com/115363604/200195278-f12bffb0-51b8-466a-8ef7-b8cf6755f41e.png)

* I will use *lvcreate* utility to create 2 logical volumes. *apps-lv* and *logs-lv*
* *apps-lv* will be used to store data for the Website while, *logs-lv* will be used to store data for logs
   - sudo lvcraete -n apps-lv -l 14G webdata-vg
   - sudo lvcreate -n logs-lv -l 14G webdata -vg
   - sudo lvs ( To verify that my Logical Volume has been created successfully )
  
![test127](https://user-images.githubusercontent.com/115363604/200195643-fd018e20-bd9f-4af8-b61e-9ee620b72817.png)

* I will verify the entire set up with the command:
  - sudo vgdisplay -v #view complete setup - VG, PV, and LV
  - suod lsblk

![test128](https://user-images.githubusercontent.com/115363604/200195744-b16adc8c-0156-446d-8379-bf70e731693f.png)

* I will format the logical volumes with *ext4* filesystem using the command:
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
 
 

  
