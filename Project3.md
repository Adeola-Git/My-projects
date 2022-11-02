
# Devops Project-3

### MEAN STACK IMPLEMENTATION

#### STEP 1: BACKEND CONFIGURATION

* I downloaded, installed, and configured Mobaxterm.
* I connected my EC2 instance to my Mobaxterm terminal
![test32](https://user-images.githubusercontent.com/115363604/198860599-f41d8df0-dd59-4a83-821f-ad2359d87d1b.png)

* Commands:
  - sudo apt update
  - sudo apt upgrade
  - curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - (To get the location of Node.js software form Ubuntu repositories)
![test33](https://user-images.githubusercontent.com/115363604/198860698-aa4ee566-ed9b-4509-af72-99d24dc8748d.png)
  
 * Commands:
   - sudo apt-get install -y nodejs (To install Node.js)
   - node -v (Verify the node installation)
   - npm -v (Verify node package installation
 
![test34](https://user-images.githubusercontent.com/115363604/198860801-0ddcf067-5f10-4699-844c-c797bbec6864.png)

* Commands:
  - mkdir Todo (Create a new directory)
  - ls (Verify the directory)
  - cd Todo (Change to the newly directory)
 
![test35](https://user-images.githubusercontent.com/115363604/198860867-dd3ce5ef-38af-42e0-9a38-85a43903b221.png)

  *  npm init (To initialise my project so that a new file named package.json will be created)

![test36](https://user-images.githubusercontent.com/115363604/198860953-0a172e93-41d4-4c9e-8dc5-5c8cb0753f7d.png)

  * ls (To confirm that I have the package.json file created)
 
![test37](https://user-images.githubusercontent.com/115363604/198860972-841bc447-006c-4340-89e7-03256a4d702f.png)

#### INSTALL EXPRESSJS

* Express helps to define routes of my application based on HTTP methods and URLs.
* Commands:
  - npm install express (Install express using npm)
  - touch index.js
  - npm install dotenv (To install the *dotenv* module)
  - vim index.js
  
![test38](https://user-images.githubusercontent.com/115363604/199542634-dd6a4118-134f-4c20-9b4f-71ed0a089ac7.png)

  - node index.js (Start my server to see if it works)
    
 * I will edit my inbound rules to open TCP port 80 and port 5000.
 
![test39](https://user-images.githubusercontent.com/115363604/199544294-3b3c32ad-2e21-4877-bfc1-95300c4c278e.png)
 * I open up my browser and try to access my servers Public IP followed ny port 5000 

![test40](https://user-images.githubusercontent.com/115363604/199544297-8c491e5e-0b9e-400b-ae0b-85ce2709e504.png)
 
 * There are three actions that my To-Do application needs to be able to do:
 * For each task I need to create *routes* that will define various endpoints that the *To-Do* app will depend on.
 * Commands:
    - mkdir routes
    - cd routes
    - touch api.js
    - vim api.js
 
![test41](https://user-images.githubusercontent.com/115363604/199545647-21d5bcd5-5e7a-4b6e-a775-e5ad2a8cf7ab.png)

#### MODELS
* A model is at the heart of JavaScript based applications, and it is what makes it interactive. We will also use models to define the database schema.
* To create a schema and a model, install *mongoose* which is a Node.js package that makes working with mongodb easier.

* Commands:
  - cd Todo
  - npm install mongoose
  - mkdir models
  - cd models
  - touch todo.js
  - vim todo.js
  
![test42](https://user-images.githubusercontent.com/115363604/199557742-07dd83b5-bc53-4f86-aef4-a280cb037950.png)

* Commands:
  - cd routes (I need to update the api.js to make use of the new model)
  - vim api.js (I will delete the code inside and paste the code below)
  
![test43](https://user-images.githubusercontent.com/115363604/199558991-aa6d87bd-8625-4b07-81f7-7d409f560537.png)

#### MONGODB DATABASE
* We need a database where I will store my data. For this I will make use of *mLab*. 

![test44](https://user-images.githubusercontent.com/115363604/199560106-e059606b-e590-4761-8121-11fa0a7c8053.png)

* I will go back to my terminal to run the below commands
* Commands:
  - cd Todo ( Change directory back to Todo)
  - touch .env ( To create a directory)
  - vi .env( To add the connection string to access the database in it, which I got from my mlab)

![test45](https://user-images.githubusercontent.com/115363604/199560784-482d297f-2b5c-4c6f-a976-a1a8643805dd.png)

* I need to update the *index.js* to reflect the use of .env so that Node.js can connect to the database
* Commands:
  - vim index.js (I will delete existing content in the file and update with the code)
  
![test46](https://user-images.githubusercontent.com/115363604/199561578-a3378c99-2158-4b2c-b9c8-0b2c19f2f0f9.png)* I will start back my server using the command:
  - node index.js
    
![test47](https://user-images.githubusercontent.com/115363604/199563027-04806fe3-389b-476d-980f-d5f548dcf953.png)

* It came back with an error so I was able to fix it with the command:
  - lsof -i tcp:5000(To get the process ID that is running )
    
![test48](https://user-images.githubusercontent.com/115363604/199563120-58450d20-b278-4769-923f-da6ebb278732.png)
    
  - kill -9 15562 ( To kill the process ID)
    
![test49](https://user-images.githubusercontent.com/115363604/199563123-0a020272-fd28-4bb6-a592-1c507849f644.png)
    
   - node index.js ( I start back my server to ensure that everything is okay)
    
![test50](https://user-images.githubusercontent.com/115363604/199563124-b38a81fa-996c-4d9e-bbb2-02339650345e.png)
  
* So far I have written backend part to my To-Do application, and configured a databse, but I do not have a frontend UI yet. I need ReactJS code to achieve that. But during development, I will need a way to test my code using RESTfull API. Therefore, I will need to make use of some API development client to test my code.
* I will use *Postmant* by installing it to test my API.
* I will then create a POST request to the API using my public IP address including the port 5000/api/todos.I will also set my header key *Content-Type* as *application/json*

![test51](https://user-images.githubusercontent.com/115363604/199565230-85f2f6a5-0ffa-40de-be13-70d6e92a0d07.png)

* I will create a GET request on my API on the public IP address including the port 5000/api/todos

![test52](https://user-images.githubusercontent.com/115363604/199565249-a134c886-5e67-4caf-8059-8039bf813fea.png)

#### STEP 2: FRONTEND CREATION
* Since I am done with the functionality I want from my backend and API, it is time to creat a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, I will use the *create-react-app* command to scaffold my app.
* Commands:
  - cd Todo
  - npx create-react-app client (It will create a new folder in my *Todo* directory called *client* where I will add all the react code)

![test53](https://user-images.githubusercontent.com/115363604/199567791-274c41ea-98bd-42e3-96de-2d791167a39a.png)
  - npm install concurrently --save-dev (To use more than one command simulteaneously fro the same terminal window)
  
![test54](https://user-images.githubusercontent.com/115363604/199567861-3ee6a4a2-b6be-4339-90fd-c73d1734dc74.png)

  - npm install nodemon --save-dev ( To run and monitor the server)

![test55](https://user-images.githubusercontent.com/115363604/199567932-7bb6c41a-d92e-4103-968d-1802191e5299.png)

  - vim package.json ( I will replace a part with the code)

![test56](https://user-images.githubusercontent.com/115363604/199568190-4bf0b0bc-ce29-4b34-a08f-9fa14ee2d840.png)

  - cd client
    -vi package.json (To add the key value pair so the application can be accessed directly from the browser)
    
![test57](https://user-images.githubusercontent.com/115363604/199569320-2ade8644-31c6-4758-a6f1-ea83e4328f37.png)
 
  - cd Todo
  - npm run dev ( My app will open and start running on *localhost:3000*)
 
![test58](https://user-images.githubusercontent.com/115363604/199569990-60f61c7f-8df8-44a8-8b87-8728dbba984f.png)
   
  * I will go to my instance to edit the inbound rulls by adding port 3000. 

![test59](https://user-images.githubusercontent.com/115363604/199570086-e547479a-7fc8-4514-854a-c1cfb3fd73d5.png)

##### CREATING MY REACT COMPONENTS
* Commands:
  - cd client
  - cd src
  - mkdir components
  - touch Input.js ListTodo.js Todo.js (I created three files inside the directory)
  - vi Input.js (To copy the code in it)
  
![test60](https://user-images.githubusercontent.com/115363604/199571370-d35bb1fc-441d-4e1f-86bf-4d0cadcbaba6.png)

* To make use of *Axios*, which is a Promise based HTTP client for the browser and node.js, I need to cd into my clinet from my terminal and run yarn add axios or npm install axios.

* Commands:
  - cd ..
  - cd .. (In client folder)
  - npm install axios
 
 ![test61](https://user-images.githubusercontent.com/115363604/199571427-0e5547c3-55a9-4f78-aaab-79a2e7adcee8.png)
 
 #### FRONTEND CREATION (CONTINUED)
 * Commands:
  - cd src/components
  - vi ListTodo.js (To paste the code into it) 

![test62](https://user-images.githubusercontent.com/115363604/199572096-67501914-3930-45a8-8440-f3aff88458e4.png)

  - vi Todo.js (To paste the code into it)
  
![test63](https://user-images.githubusercontent.com/115363604/199572194-afedb412-d0a9-4b2f-8c61-48bdb549c771.png)

  - cd ..(Changed directory back to src folder)
  - vi App.js (To paste the code into it)
  
![test64](https://user-images.githubusercontent.com/115363604/199572397-3f3b1f31-b5d0-4d6c-aeb7-82471bb6a468.png)

  - vi App.css (To paste the code into it)
  
![test65](https://user-images.githubusercontent.com/115363604/199586266-95f1aab9-9885-41d7-a593-ba08cb99c2ac.png)

  - vi index.css (To paste the code into it)

![test66](https://user-images.githubusercontent.com/115363604/199586330-152ee9a2-a440-4228-a24f-47485e23550e.png)

  - cd ../.. (To change diectory back to Todo)
  - npm run dev ( Assuming no errors when saving all these files, my To-Do app should be ready and fully functional. Creating a task, deleting a task and viewing all my task)
  
![test67](https://user-images.githubusercontent.com/115363604/199586373-ee2bb357-abfe-48dd-8e92-3685162af7cd.png)

* I will go back to my web broswer and refresh it. It will look thus:

![test68](https://user-images.githubusercontent.com/115363604/199586515-fe42c11f-7486-4c18-85aa-3a5b870ed46f.png)



 
  

  
 

  
  
