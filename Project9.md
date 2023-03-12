
# Devops Project-9

## TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

### INSTALL AND CONFIGURE JENKINS SERVER

#### Step 1 – Install Jenkins server
* Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
* Check that the load balancer on the browser to ensure that its is working fine.
* Connect to the new installed server *jenkins* via ssh.
* Install JDK (since Jenkins is a Java-based application)
  - sudo apt update
  - sudo apt install default-jdk-headless

* Install Jenkins
  - wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
  - sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
  - sudo apt update
  - sudo apt-get install jenkins ( To make sure jenkins is up and running )

![new66](https://user-images.githubusercontent.com/115363604/224514728-ebee60af-0824-426e-ac60-f88f6af83a51.png)

* By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group.

![new69](https://user-images.githubusercontent.com/115363604/224514975-60e4ff6d-b5f5-4525-89e2-14f2be6f619c.png)

* Perform initial Jenkins setup.
* From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
* You will be prompted to provide a default admin password

![new67](https://user-images.githubusercontent.com/115363604/224514840-43f4cde7-21b6-471a-be8d-6464dad67cb9.png)
  
* Retrieve it from your server:
  - sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  
![new70](https://user-images.githubusercontent.com/115363604/224515102-7aa4b04b-2863-4235-b1f6-cebf15f93122.png)

* Then you will be asked which plugings to install – choose suggested plugins.

![new71](https://user-images.githubusercontent.com/115363604/224515105-08d6dc9f-a2f9-4d2d-8d18-275c7f7e2388.png)

* Once plugins installation is done – create an admin user and you will get your Jenkins server address.
* The installation is completed!
![new72](https://user-images.githubusercontent.com/115363604/224515161-aac43e64-b509-49eb-82c0-cf048eec282f.png)
  
![new73](https://user-images.githubusercontent.com/115363604/224515163-0825707f-4666-439a-b33e-b446dd128a2a.png)

#### Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks.
  
* In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

* Enable webhooks in your GitHub repository settings
  
![new74](https://user-images.githubusercontent.com/115363604/224515256-9333e194-0694-49d1-9413-3923ca2a1724.png)

* Go to Jenkins web console, click "New Item" and create a "Freestyle project"
* To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself
* In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so   Jenkins could access files in the repository.
* Go to Source code management.
* Grab your source code from your git hub and paste it in it.
* Go to configure Global security
* Enable proxy compatibility
* Apply and save.
  
* Go to the *jenkins* terminal to retart it
  - systemctl restart jenkins
  
![new75](https://user-images.githubusercontent.com/115363604/224515493-0d2565d5-597c-4fa5-a172-d884f9f11706.png)

* Go back to jenkins
* Save the configuration and let us try to run the build. For now we can only do it manually.
  Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it.
  
![new102](https://user-images.githubusercontent.com/115363604/224518330-cd116c14-b933-4c5f-bd2c-be1e4bdf676c.png)


* Click "Configure" your job/project and add these two configurations
* Configure triggering the job from GitHub webhook:
* Go to build trigger, Github hook trigger for GITscm polling.
  
* Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".
* Go to Build, Add build setup, Archive the artifacts, Post-build-Actions and *save*.

* Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.
  
![new77](https://user-images.githubusercontent.com/115363604/224515851-47f0b8ea-7bb4-49d4-a095-0b67adbb8067.png)

* You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.
  
![new78](https://user-images.githubusercontent.com/115363604/224515950-303ae442-1b48-4b50-be2b-6df39a30e7ec.png)
  
![new103](https://user-images.githubusercontent.com/115363604/224518414-b17fc44d-8fbc-4113-8380-d7284d5fba9a.png)
  
* You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.
* By default, the artifacts are stored on Jenkins server locally
  - sudo ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
 
![new80](https://user-images.githubusercontent.com/115363604/224516153-a318da96-9879-4a45-9e35-5b5535158f0f.png)

### CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH
  
#### Step 3 – Configure Jenkins to copy files to NFS server via SSH
  
* Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.
* Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "Publish Over SSH".
* Install "Publish Over SSH" plugin.
  - On main dashboard select "Manage Jenkins" and choose "Manage Plugins" menu item.
  - On "Available" tab search for "Publish Over SSH" plugin and install it.
  
![new81](https://user-images.githubusercontent.com/115363604/224516486-10ac08d1-7552-4712-93d1-d0f33ccd166b.png)

* Configure the job/project to copy artifacts over to NFS server.
* On main dashboard select "Manage Jenkins" and choose "Configure System" menu item.
* Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:
  - Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)
    Arbitrary name
  - Hostname – can be private IP address of your NFS server
  - Username – ec2-user (since NFS server is based on EC2 with RHEL 8)
  - Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server.
  
* Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

![new82](https://user-images.githubusercontent.com/115363604/224516464-3d2e31bc-1bd7-4e2a-ab53-d4471ff716e6.png)

  
* It came out with an error so I will have to create a different pairing to generate a new private key with .PEM on the *jenkins server* and the copy and paste it into the *NFS server* .shh/authorized_keys file.
* Go to jenkins server and run the command:
  - ssh-keygen -t ecdsa -b 521 -m PEM 
  - cd .ssh
  - cat id_ecdsa.pub key ( Copy and paste it into the nfs .ssh/authorized_keys )
  - cat id_ecdsa key ( Copy and paste it into the jenkins SSH plugin configuration section )
  
![new83](https://user-images.githubusercontent.com/115363604/224516658-df8ba27a-c04c-450a-9b22-6d913acded4f.png)
  
![new84](https://user-images.githubusercontent.com/115363604/224516661-906a63b1-bf9e-4421-a9ea-b8177519c30b.png)
  
* Go to *NFS* server and run the command
  - cd .ssh/authorized_keys 
  
![new85](https://user-images.githubusercontent.com/115363604/224516841-a34534aa-48a7-40f0-b40d-141fe803f63f.png)
  
![new86](https://user-images.githubusercontent.com/115363604/224516843-deb24fbd-d8b1-4a41-a7d7-91362e979604.png)

* Go back to *jenkins* paste the .PEM key into the provide private key and fill out the rest, then test the configuration.
  
![new87](https://user-images.githubusercontent.com/115363604/224516911-051f06d8-e5c2-4cfc-916a-74aab22ad216.png)

* Save the configuration, open your Jenkins job/project configuration page and add another one "Post-build Action". 
* Configure it to send all files probuced by the build into our previouslys define remote directory. In our case we want to copy all files and directories – so we use   **.
* Add and click on Select build actifact over ssh, go to source files and input (**), then *save*.
  
* Save this configuration and go ahead, change something in README.MD file in your GitHub Tooling repository.
  
![new89](https://user-images.githubusercontent.com/115363604/224517070-95623649-2820-42c7-9b31-05cb0dc5b293.png)

* Webhook will trigger a new job and in the "Console Output" of the job. It came out with an error.

![new101](https://user-images.githubusercontent.com/115363604/224518313-0874d697-2680-4735-9a9a-76aa1b9a65be.png)


* Go back to the *nfs server* and check the permission using the command:
  - ll /mnt/apps
  - sudo chmod -R 777 /mnt/apps
  - sudo chown -R nobody:nobody /mnt/apps
  
![new91](https://user-images.githubusercontent.com/115363604/224517184-57480702-1776-48e1-a8c2-c37e7821832e.png)

* Go back to *jenkins* and rebuild.
  
![new100](https://user-images.githubusercontent.com/115363604/224518257-820464e4-bef2-432e-ae83-0664299222b9.png)


* To make sure that the files in /mnt/apps have been udated – connect via SSH/Putty to your NFS server and check README.MD file
  - cat /mnt/apps/README.md

![new93](https://user-images.githubusercontent.com/115363604/224517313-901e33d8-c003-4f34-8fc2-00c35a74b5b5.png)

  
  

  
  
  
