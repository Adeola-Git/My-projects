
# Devops Project-9

## ANSIBLE CONFIGURATION MANAGEMENT – AUTOMATE PROJECT 7 TO 10

This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML.

Let us get started!

#### Ansible Client as a Jump Server (Bastion Host)
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.


![bastion](https://user-images.githubusercontent.com/115363604/227682127-88b47811-a6bb-486a-9c2e-174c9b075da8.png)


#### Task

* Install and configure Ansible client to act as a Jump Server/Bastion Host
* Create a simple Ansible playbook to automate servers configuration

## INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE
1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

![pro3](https://user-images.githubusercontent.com/115363604/227685668-1c198ffd-4644-4ee3-b84d-40214d02ce09.png)

2. In your GitHub account create a new repository and name it ansible-config-mgt.

![pro4](https://user-images.githubusercontent.com/115363604/227685959-0d92c516-76ce-40ab-98ca-10d799cc5e61.png)

3. Install Ansible
    - sudo apt update
    - sudo apt install ansible
    - ansible --version ( To check your Ansible version )

![pro2](https://user-images.githubusercontent.com/115363604/227683361-5311e6f7-d544-4003-9952-6fe1a611905b.png)

4. Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

* Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

![pro5](https://user-images.githubusercontent.com/115363604/227686842-f360963f-adda-47ec-a3d3-1e5891612e0c.png)

* Configure Webhook in GitHub and set webhook to trigger ansible build.

![pro6](https://user-images.githubusercontent.com/115363604/227687338-7257e9df-0046-42bb-9ca6-433b3a7f967b.png)

* Configure a Post-build job to save all (**) files, like you did it in   Project 9.

![pro7](https://user-images.githubusercontent.com/115363604/227687648-97a9ccc9-0e45-465b-82a8-ee94babc50ed.png)

5. Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder
    - ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

![pro8](https://user-images.githubusercontent.com/115363604/227688025-97b71abd-b23c-4ae2-af1c-4597f6bee33a.png)

![pro9](https://user-images.githubusercontent.com/115363604/227688029-1dc5d4c6-4fb7-4ded-9b82-9c51075d8f55.png)

Note: Trigger Jenkins project execution only for /main (master) branch.

Now your setup will look like thi!

![Architecture](https://user-images.githubusercontent.com/115363604/227688376-51b8a126-912c-41b8-a4e0-b3ee6d5d9c4b.png)

Tip Every time you stop/start your Jenkins-Ansible server – you have to reconfigure GitHub webhook to a new IP address, in order to avoid it, it makes sense to allocate an Elastic IP to your Jenkins-Ansible server (you have done it before to your LB server in Project 10). Note that Elastic IP is free only when it is being allocated to an EC2 Instance, so do not forget to release Elastic IP once you terminate your EC2 Instance.

#### Step 2 – Prepare your development environment using Visual Studio Code

1. First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging              comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – Visual Studio Code (VSC), you can get it here.

2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.

3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

    - git clone <ansible-config-mgt repo link>
    
![pro10](https://user-images.githubusercontent.com/115363604/227689064-ce815b91-1dd0-4fe8-beff-0ca242dfea7b.png)

#### BEGIN ANSIBLE DEVELOPMENT
1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.
2. Checkout the newly created feature branch to your local machine and start building your code and directory structure
    
![pro11](https://user-images.githubusercontent.com/115363604/227689132-7d7f511a-f985-4837-b705-0a14a851d47d.png)
    
3. Create a directory and name it playbooks – it will be used to store all your playbook files.
    
![pro12](https://user-images.githubusercontent.com/115363604/227689399-1d9b9e6a-9f91-46e8-8b36-2dc92a757dac.png)
    
4. Create a directory and name it inventory – it will be used to keep your hosts organised.
    
![pro13](https://user-images.githubusercontent.com/115363604/227689407-692bbb8b-ddc6-438d-90a7-b73de5bea618.png)

5. Within the playbooks folder, create your first playbook, and name it common.yml
    
![pro14](https://user-images.githubusercontent.com/115363604/227689478-1ae2c6a6-7f3f-48f1-acfe-42e343d29395.png)
    
6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.
    
![pro15](https://user-images.githubusercontent.com/115363604/227689481-14ca320d-cb2b-455b-a553-cfc771914bd3.png)

#### Step 4 – Set up an Ansible Inventory
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:
    
    - eval `ssh-agent -s`
    
![pro16](https://user-images.githubusercontent.com/115363604/227689668-cbf09922-1581-4cda-8664-1fe5dd72afa7.png)
    
    - ssh-add <path-to-private-key>

![pro20](https://user-images.githubusercontent.com/115363604/227689945-cd168b59-6dc1-4937-90e1-b76989ae1999.png)

* Confirm the key has been added with the command below, you should see the name of your key
    - ssh-add -l

![pro21](https://user-images.githubusercontent.com/115363604/227689983-85ba59f2-4489-47e4-a55e-fbdac962117f.png)
    
* Now, ssh into your Jenkins-Ansible server using ssh-agent
    - ssh -A ubuntu@public-ip
    
![pro19](https://user-images.githubusercontent.com/115363604/227689836-04b98828-94bb-410d-b613-c8c8a7eb6935.png)

* Update your inventory/dev.yml file with this snippet of code:
    
![pro22](https://user-images.githubusercontent.com/115363604/227690061-80b70d8a-9ba7-4305-ad38-f6df01ee7b74.png)


    
    
