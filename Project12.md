
# Devops Project-9

## ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)

In this project you will continue working with ansible-config-mgt repository and make some improvements of your code. Now you need to refactor your Ansible code, create assignments, and learn how to use the imports functionality. Imports allow to effectively re-use previously created playbooks in a new playbook – it allows you to organize your tasks and reuse them when needed.

### Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

In your case, you will move things around a little bit in the code, but the overal state of the infrastructure remains the same.

Let us see how you can improve your Ansible code!

#### Step 1 – Jenkins job enhancement
Before we begin, let us make some changes to our Jenkins job – now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place. Besides, it consumes space on Jenkins serves with each subsequent change. Let us enhance it by introducing a new Jenkins project/job – we will require Copy Artifact plugin.

1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.
  - sudo mkdir /home/ubuntu/ansible-config-artifact
 
2. Change permissions to this directory, so Jenkins could save files there – chmod -R 0777 /home/ubuntu/ansible-config-artifact

![m1](https://user-images.githubusercontent.com/115363604/228620490-6e12b07d-f7e8-4c81-8239-c31e2bb1931a.png)

3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

![m2](https://user-images.githubusercontent.com/115363604/228620965-c4b16b27-3edd-4750-9905-fcab678a8866.png)

![m3](https://user-images.githubusercontent.com/115363604/228620985-76a28a88-c9ef-450b-bdac-4ed64ea3d10a.png)

4. Create a new Freestyle project (you have done it in Project 9) and name it save_artifacts.

![m4](https://user-images.githubusercontent.com/115363604/228621500-f29b13b4-bf8a-45fd-b508-69d3b5c8b47d.png)

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:

![m5](https://user-images.githubusercontent.com/115363604/228622340-ea5797f2-f52d-49c7-a583-de9911c5a295.png)

![m6](https://user-images.githubusercontent.com/115363604/228622383-bea5e010-e7b2-4f4b-9aca-f03c5cb47607.png)

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, create a Build step and choose Copy artifacts from other project, specify ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

![m7](https://user-images.githubusercontent.com/115363604/228622398-67d27730-e6fb-4d1e-a0ff-12f0485b8f78.png)

7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside master branch).

If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

Now your Jenkins pipeline is more neat and clean.

![m8](https://user-images.githubusercontent.com/115363604/228623467-c4da9ebb-7785-4564-b694-23f6e0ad19e7.png)


### REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML

#### Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.

![M14](https://user-images.githubusercontent.com/115363604/228627926-6c921d6b-201f-4010-88c3-07fdcec59fc1.png)

DevOps philosophy implies constant iterative improvement for better efficiency – refactoring is one of the techniques that can be used, but you always have an answer to question "why?". Why do we need to change something if it works well?

In Project 11 you wrote all tasks in a single playbook common.yml, now it is pretty simple set of instructions for only 2 types of OS, but imagine you have many more tasks and you need to apply this playbook to other servers with different requirements. In this case, you will have to read through the whole playbook to check if all tasks written there are applicable and is there anything that you need to add for certain server/OS families. Very fast it will become a tedious exercise and your playbook will become messy with many commented parts. Your DevOps colleagues will not appreciate such organization of your codes and it will be difficult for them to use your playbook.

Most Ansible users learn the one-file approach first. However, breaking tasks up into different files is an excellent way to organize complex sets of tasks and reuse them.

Let see code re-use in action by importing other playbooks.

1. Within playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously. Dont worry, you will understand more what this means shortly.

2. Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored. This is merely for easy organization of your work. It is not an Ansible specific concept, therefore you can choose how you want to organize your work. You will see why the folder name has a prefix of static very soon. For now, just follow along.

3. Move common.yml file into the newly created static-assignments folder.

4. Inside site.yml file, import common.yml playbook.

![m9](https://user-images.githubusercontent.com/115363604/228624819-4bb16c26-43f2-4d13-9667-279dbe97f228.png)

The code above uses built in import_playbook Ansible module.

Your folder structure should look like this;

![m10](https://user-images.githubusercontent.com/115363604/228625719-1d6ba9f3-575b-4b35-989c-27f16dd7ddb7.png)

5. Run ansible-playbook command against the dev environment

Since you need to apply some tasks to your dev servers and wireshark is already installed – you can go ahead and create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.

![m11](https://user-images.githubusercontent.com/115363604/228626325-e30106c0-497d-4ba7-9a96-0c7250e813fa.png)

Update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers:

![m12](https://user-images.githubusercontent.com/115363604/228626690-c77df2e6-0bd8-49c7-a797-0565fbfff017.png)

  - cd /home/ubuntu/ansible-config-mgt/

  - ansible-playbook -i inventory/dev.yml playbooks/site.yml

![M13](https://user-images.githubusercontent.com/115363604/228627423-ad45cc22-b451-4279-b2f9-52e21f1099e5.png)

Make sure that wireshark is deleted on all the servers by running wireshark --version

We will check if the wireshark has been deleted from two of the servers *nfs* and *db*

![M15](https://user-images.githubusercontent.com/115363604/228628475-ecd3db09-e2dd-45af-bbc1-463317fb5fa8.png)

![M16](https://user-images.githubusercontent.com/115363604/228628490-6bd69b2e-f558-4787-b2bc-a81415b0ec6d.png)


Now you have learned how to use import_playbooks module and you have a ready solution to install/delete packages on multiple servers with just one command.

### CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’

#### Step 3 – Configure UAT Webservers with a role ‘Webserver’
We have our nice and clean dev environment, so let us put it aside and configure 2 new Web Servers as uat. We could write tasks to configure Web Servers in the same playbook, but it would be too messy, instead, we will use a dedicated role to make our configuration reusable.

1. Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UAT.

Tip: Do not forget to stop EC2 instances that you are not using at the moment to avoid paying extra. For now, you only need 2 new RHEL 8 servers as Web Servers and 1 existing Jenkins-Ansible server up and running.

2. To create a role, you must create a directory called roles/, relative to the playbook file or in /etc/ansible/ directory.

![m17](https://user-images.githubusercontent.com/115363604/229570869-71807b10-7511-497b-b5ef-802d613a8a53.png)

The entire folder structure should look like below, but if you create it manually – you can skip creating tests, files, and vars or remove them if you used ansible-galaxy

![m18](https://user-images.githubusercontent.com/115363604/229571540-2c6b5bfc-0225-4cb9-a7bb-a7c8d93c8ff7.png)

3. Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers
NOTE: Ensure you are using ssh-agent to ssh into the Jenkins-Ansible instance just as you have done in project 11;

![m19](https://user-images.githubusercontent.com/115363604/229571903-38a55834-25dd-458f-8701-b04700e265b1.png)

4. In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.

![image](https://user-images.githubusercontent.com/115363604/229576647-5d6a06cb-66fb-4df8-92a8-3a42bcfb90c8.png)

5. It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

  - Install and configure Apache (httpd service)
  - Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.

![m24](https://user-images.githubusercontent.com/115363604/229575149-4d997a4d-c45b-4b8f-9fc6-d84e262c126c.png)

  - Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
  - Make sure httpd service is started

Your main.yml may consist of following tasks:
  
![m21](https://user-images.githubusercontent.com/115363604/229572640-883dad41-d1c6-4cb4-bf3b-53e0c57e2004.png)

### REFERENCE WEBSERVER ROLE
  
#### Step 4 – Reference ‘Webserver’ role
Within the static-assignments folder, create a new assignment for uat-webservers uat-webservers.yml. This is where you will reference the role.
  
Remember that the entry point to our ansible configuration is the site.yml file. Therefore, you need to refer your uat-webservers.yml role inside site.yml.

So, we should have this in site.yml
  
![m22](https://user-images.githubusercontent.com/115363604/229573395-e6248e24-5016-4185-89ed-c2643703ca43.png)

#### Step 5 – Commit & Test
Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your Jenkins-Ansible server into /home/ubuntu/ansible-config-mgt/ directory.
  
![m23](https://user-images.githubusercontent.com/115363604/229574699-6f5de6b9-4697-43dc-88c6-ab6273d93c34.png)

Now run the playbook against your uat inventory and see what happens:
  
  - sudo ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yaml  

![m25](https://user-images.githubusercontent.com/115363604/229575729-7ae5c8c6-2da3-4c64-aa9d-4332a33964f6.png)
  
You should be able to see both of your UAT Web servers configured and you can try to reach them from your browser:

http://<Web1-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php

or

http://<Web1-UAT-Server-Public-IP-or-Public-DNS-Name>/index.php
  
![m26](https://user-images.githubusercontent.com/115363604/229576072-74020762-fd29-4651-92e5-ad0e5eecb7e7.png)

![m27](https://user-images.githubusercontent.com/115363604/229576095-60a8087e-add3-48b7-b974-c7b9700cc82a.png)

Your Ansible architecture now looks like this:

![project12_architecture](https://user-images.githubusercontent.com/115363604/229575958-150b48cf-ef4d-490e-bcd7-6d0df0b87e77.png)
