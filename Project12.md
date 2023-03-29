
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





