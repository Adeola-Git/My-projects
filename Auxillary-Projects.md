
# Devops Aux-Project-1

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


  - 
  
