# Devops Project-4
## MEAN STACK DEPLOYMENT TO UBUNTU IN AWS
#### STEP 0: PREPARING PREREQUISITES
* I will create my virtural server with Ubuntu OS from my AWS.

##### TASK
* In this assignment I am going to implement a simple Book Register web form using MEAN stack.

#### STEP 1: INSTALL NODEJS
* *Node.js* is a JavaScript runtime built on Chrome's V8 JavaScript engine. *Node.js* 
* Commands:
  - sudo apt update ( I need to update the server being a newly deployed server)
  - sudo apt upgrade ( I need to upgrade the server being a newly deployed server)
  - sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates (To add cerficates)
  - curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

![test69](https://user-images.githubusercontent.com/115363604/199594526-e4105b66-02c8-4994-8474-c9f8e051b2d4.png)

  - sudo apt install -y nodejs

![test70](https://user-images.githubusercontent.com/115363604/199599693-be3ab617-8e00-4fc0-ae89-687789420330.png)

#### STEP 2: INSTALL MONGODB
* MongoDB stores data in flexible, *JSON-like* documents. Fields in a database can vary from document to document and data structure can be changed over time. For my exmaple I am adding book records to MongoDB that contain book name, isbn number, author, and number of pages
* Commands:
  - sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
  - echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

![test71](https://user-images.githubusercontent.com/115363604/199600540-fe90f551-8351-4576-a60f-46b6de4053ab.png)

* ##### INSTALL MONGODB
* Commands:
  - sudo apt install -y mongodb
  - sudo service mongodb start ( Start the server)
  - sudo systemctl status mongodb  Verity that the service is up and running)

![test72](https://user-images.githubusercontent.com/115363604/199601190-7e5753d6-2c71-4cf3-81b2-958305d197e8.png)

  - sudo apt install -y npm (Install *npm*)
  - sudo apt install body-parser (I need a *body-parser* package to help me process JSON files passed in requests to the server)
  
![test73](https://user-images.githubusercontent.com/115363604/199601637-de90e8d9-1122-453f-8fca-fadaa3e83a81.png)
 
 - mkdir Books && cd books (To create a folder names *Books* and change driectory into it)
 - npm init (To initialize npm project

![test74](https://user-images.githubusercontent.com/115363604/199602260-c4467a13-7276-45ef-8a69-ee30f09c4c69.png)

 - vi server.js (To add a file named *server.js* and copy the web server code into it)

![test75](https://user-images.githubusercontent.com/115363604/199602327-831e76d3-1d94-4e14-ac24-df8ed5be7469.png)

#### STEP 3: INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER
* Express is a minimal and flexible *Node.js* web application framework that provides features for web and mobile applications.
* Commands:
  - sudo npm install express mongoose (To install mongoose package which will be used to establish a schema for the database to store data in my book register)
  - mkdir apps && cd apps 
  - vi routes.js (To create the file *routes.js* and paste the code into it)
  
![test76](https://user-images.githubusercontent.com/115363604/199603137-2e164082-0c24-47d9-bd29-97c55d0bb09f.png)
  
  - mkdir models && cd models (Create a folder named *models* and change directory into it)
  - vi book.js ( Create a file names *book.js* and paste the code into it)
  
![test77](https://user-images.githubusercontent.com/115363604/199603478-2fcf14da-09ba-44a1-90fd-7d2eb8e37ce7.png)

#### STEP 4: ACCESS THE ROUTES WITH *ANGULARJS*
* AngularJS provides a web framework for creating dynamic views in my web applications.
* Commands:
  - cd ../.. (Change directory back to *Books*)
  - mkdir public public && cd public (Create a folder named public and change directory into it)
  - vi scripts.js (Add a file named *scripts.js* and paste the code into it)
  
![test78](https://user-images.githubusercontent.com/115363604/199604412-911cb9ae-d1f2-40ad-852a-42b8177d7905.png)

  - vi index.html (Create the file and change directory into it)

![test79](https://user-images.githubusercontent.com/115363604/199604713-5891bbc3-7dce-4e75-b766-dd2adaa4a188.png)

  - cd .. (Change directory back to *Books*)
  - node server.js (To start back the server)

![test80](https://user-images.githubusercontent.com/115363604/199605321-35f4b69c-2284-46cb-bbe7-4cb3820b2417.png)

* I will go to my AWS instance to open port 3300 in my inbound security. Then I will open another terminal to test it

![test81](https://user-images.githubusercontent.com/115363604/199605395-31755ccd-8630-48a3-ba23-4d4223755f5a.png)

  - curl -s http://localhost:3300 (It will return an HTML page, it is hardly readable in the CLI, but I can also try and access it from the internet)
 
![test82](https://user-images.githubusercontent.com/115363604/199605458-33d68e26-e8eb-43a2-91ee-4ded690cf97d.png)

* I will go to my web browser to access it using my public IP address with the port 3300

![test83](https://user-images.githubusercontent.com/115363604/199606081-58693f96-5496-4a6e-9afb-11c46129d6e9.png)

* I will input the name, isbn, Author and pages to test it

![test84](https://user-images.githubusercontent.com/115363604/199606164-4541f5a3-e557-4ae2-9579-4cb77c846632.png)

