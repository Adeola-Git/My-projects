
# Devops Auxillary-Projects

## SHELL SCRIPTING

* I will onboard 20 new linux users onto a server. 
* Create a shell scripts that reads a *csv* file that contains the first name of the users to be onboarded
* I will open my git bash terminal, change directory to my download folder where my *pem-key* is.
* Commands:
  - vi onboard.sh (To create aa new file where i will paste the script)
  
![test85](https://user-images.githubusercontent.com/115363604/200187711-ae8c8e2f-ac89-4285-9913-0eb4c91e073e.png)

* I will then copy my *onboard.sh* file to my ubuntu instance using the command:
  - scp -i test-key.pem onboard.sh ubuntu@3.89.110.8
  
![test86](https://user-images.githubusercontent.com/115363604/200187857-8c933123-e3a8-45e8-ba03-a32a07a1ac74.png)

* I will connect to my EC2 instance and check if the onboard.sh folder is there with the command:
   - ls -l

![test87](https://user-images.githubusercontent.com/115363604/200188154-437fcbf9-6ccf-4348-8961-1b630de5479b.png)

* I need to make a folder I will use to onboard my 20 new Linux users
* Commands: 
  - mkdir Shell ( Create the project folder called Shell )
  - cd Shell ( Move into the Shell folder )
  - touch names.csv ( Create a *csv* file name *names.csv* )
  - vim names.csv ( I will insert some random names into it

![test88](https://user-images.githubusercontent.com/115363604/200188308-82fb00eb-af1d-4133-b2df-fa1b22461d82.png)

* I will create a file for my public key
  - touch id_rsa pub
  - vi id_rsa.pub ( To paste in the public key )

![test89](https://user-images.githubusercontent.com/115363604/200188732-8af22979-6ec7-4574-8dc4-04d3f6878225.png)

* Commands:
  - touch id_rsa
  - vi id_rsa ( To paste in the private key )

![test90](https://user-images.githubusercontent.com/115363604/200188757-dd1ad5ce-50b4-4ab4-bc5e-6c80dd3d6cef.png)

* I will edit and change some parameters in my *onborad.sh* so I can include my home directory in it
  - vi onboard.sh

![test91](https://user-images.githubusercontent.com/115363604/200188852-046fac0f-d9dc-4433-8586-bd874d16e9da.png)

* I need to create a group that all my users will be added to and give the script folder necessary permissions
* Commands:
  - sudo groupadd developers
  - sudo chmode +x onboard.sh

![test92](https://user-images.githubusercontent.com/115363604/200189019-83613547-d78a-4a34-bf63-a85477b58da9.png)
 
 * Commands:
   - ./onboards.sh (To run the script by changing to user root )

![test93](https://user-images.githubusercontent.com/115363604/200190292-01d5409f-1be1-415c-b41e-d4fa2eaaeb76.png)

* Commands:
  - ls -l/home (To check if the names have been created )
  
![test94](https://user-images.githubusercontent.com/115363604/200189173-3e1d4395-5deb-4b5f-b033-fbf746cea46d.png)

* Commands: 
  - getent group developers ( To check if the group account was created )
  - cat /etc/passwd ( To see the details of the users created )

![test95](https://user-images.githubusercontent.com/115363604/200189281-4804ebb3-26bb-46af-9afe-b899d0ef23e4.png)

* To test the users I created I will open a new terminal, change to the downloads folder and run the commands:
  - touch aux-proj.pem ( To create a file )
  - vi aux-proj.pem ( To paste the private key into it )

![test96](https://user-images.githubusercontent.com/115363604/200189431-952b28bb-04c4-466c-8c63-d8540422c547.png)

* I will to *ssh* into one of my users account to see if I will be able to log
  - ssh -i aux-proj.pem Adeola@3.89.110.8

![test97](https://user-images.githubusercontent.com/115363604/200189552-c9b06a82-58df-44a8-bf96-8b76f8aa0c20.png)
 
* The command gave an error so I need to change the permission to be able to run the command:
  - is -l | grep aux-proj.pem ( To check the permission that the file has )
  - sudo chmod 600 aux-proj.pem ( To give the permission I need )
  - ls -l | grep aux-proj.pem ( To check if the permission has been changed )

![test98](https://user-images.githubusercontent.com/115363604/200189686-10c871a3-2fff-45e2-ba7a-5dd1d5ab51bb.png)

* I will run the command again to see if I am able to log in this time 
  - ssh -i aux-proj.pem Adeola@3.89.110.8

![test99](https://user-images.githubusercontent.com/115363604/200189784-b42ba530-f37c-457c-9c47-5feedb807cb8.png)

* Commands:
  - sudo apt update ( To check if the user have sudo privilege )

![test100](https://user-images.githubusercontent.com/115363604/200189852-eefaf94d-716a-4fad-a184-72e510a1f4ff.png)

* Commands:
  - ls -la ( To list what I have in the home directory )
  - ls -la .ssh/ ( To list what I have in the directory )

![test101](https://user-images.githubusercontent.com/115363604/200189944-ece39565-d496-4956-81f1-3a04f51a7e2b.png)

* I will log in with another user
  - ssh -i aux-project Samuel@3.89.110.8

![test102](https://user-images.githubusercontent.com/115363604/200190108-f70ad963-3a77-45b3-b5db-5ece0d0f294c.png)

* Commands:
  - sudo apt update ( To check if the user have sudo privilege ) 
  
![test103](https://user-images.githubusercontent.com/115363604/200190128-394fa3c3-470b-4961-9001-e0a5369290b1.png)

* Commands:
  - ls -la ( To check what I have in the home directory )
  - ls -la .ssh/ ( To list what I have in the directory )

![test104](https://user-images.githubusercontent.com/115363604/200190141-9f58dc8a-9d09-4874-86df-322d39201e21.png)

