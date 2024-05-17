## MERN STACK IMPLEMENTATION IN AWS 
### Introduction:
The MERN stack is a collection of technologies that help developers build robust and scalable web applications using JavaScript. The acronym “MERN” stands for MongoDB, Express, React, and Node.js, with each component playing a role in the development process. MongoDB serves as a document-oriented database that can efficiently store data in JSON format.

**This guide shows comprehensive overview on setting up and utilizing each components of MERN stack in creating and developing robust web applications**

## Step 0: Prerequisites

**1.** EC2 Instance of t3.small type and Ubuntu 24.04 LTS (HVM) was lunched in the us-east-1 region using the AWS console. The choice of the instance type was based on the following:

- Memory: The t3.small instance offers more memory than the t2.micro, which is advantageous for applications that require more memory to operate efficiently.

- Burst Capability: While both instances offer burstable CPU performance, the t3 instances have a more flexible burst model, allowing for more sustained performance during burst periods. This is important for workloads that require consistent performance over longer periods.

- Performance: While both instances offer burstable performance, the t3.small typically provides better baseline performance compared to the t2.micro. This might be necessary for applications that require a bit more processing power.

![Setting up EC2 instance](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/adcb7061-eb9c-44e6-8446-fbad9ce53a3d)
![Network info](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/42671bb4-8b87-4b77-8633-787c4eb06a70)


**2.** Attached SSH key named my-secrete-key to access the instance on port 22

**3.** The security group was configured with the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.

- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.

- Allow traffic on port 5000 (Custom TCP) with source from anywhere.

- Allow traffic on port 3000 (Custom TCP) with sourec from anywhere.

![Security Groups](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/cf0f5282-8f4d-4317-b0d0-ae635f8f7d9e)

**4.** The ssh key permission was changed and connected to the EC2 instance running
```
chmod 400 my-secrete-key
```
```
ssh -i "my-secrete-key.pem" ubuntu@54.80.221.206
```
![ssh connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7b338fac-6736-402c-afaf-33df9192719f)

## Step 1 - Backend Configuration

**1. Update and Upgrade package index**
```
sudo apt update && sudo apt upgrade -y
```
![package update and upgrade](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/0d8596e7-3283-4a51-a47c-ecd8c166e51a)

**2. Get the location of Node.js software from the repositories**
```
curl fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![checking nodejs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/59c002d8-265f-4353-8749-c2ecb4c9aefb)

**3. Install Node.js with the command from the terminal**
```
sudo apt-get install nodejs -y
```
![nodejs installed](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e16da5fc-06ee-4e9c-b069-c7cd5b9f5bec)

**4. Verify the node installation**
```
node -v
npm -v
```
### Application Code Setup

**1. Create a new directory for the todo project**
```
mkdir Todo
```
```
ls
```
**2. Change the current directory to the newly created**
```
cd Todo
```
And type:
```
npm innit
```
To initialize the project and create a new file named ```package.jason```. You can press "Enter" several times to reveal and accept default values and type "yes" to write out the ```package.jason``` file.

![Creating a new directory Todo](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/85ffcce1-b30e-4b75-850d-b4d9501f08d9)

### Install ExpressJS

Express.js, or simply Express, is a back end web application framework for building RESTful APIs with Node.js, released as free and open-source software under the MIT License. It is designed for building web applications and APIs.

**1. Install ExpressJS using npm**
```
npm install express
```
![Installing express](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/35ddff11-433e-42b0-9b4f-ab7e33ac3ada)

**2. Create a file with the command below**
```
touch index.js && ls
```
![touch indexjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3d14bbc2-d12b-41f0-ba4d-101ebb1cdca6)

**3. Install the dotnev module**
```
npm install dotenv
```
**4. Open index.js file with the command below**
```
vim index.js
```
Type the following code below:
```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
![indexjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/25030aaf-d3c9-4721-85d7-85f8969fbcc4)

**Note:** Port 5000 have been specified to be used in the code. This was required later on the browser.

**5. Start the server to see if it works. Open your terminal in the same directory as your index.js file. Run**
```
node index.js
```
![node js server running](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f78d6a7b-ade5-4094-95a3-432b83b3f89b)

Port 5000 has been opened in the EC2 security group

**Access the server's public DNS name with 5000**
```
https://ec2-54-80-221-206.compute-1.amazonaws.com:5000
```
![Welcome to Express](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3696992e-91e8-4a33-b304-d932e76d394e)

### Routes
There are three actions that the ToDo application needs to be able to do:

- Create a new task
- Display list of all task
- Delete a completed task

Each task was associated with some particular endpoint and used different standard HTTP request methods: POST, GET, DELETE.

For each task, routes were created which defined various endpoints that the ToDo app depends on.

**1. Create a folder routes, switch to routes directory and create a file api.js. Open the file**
```
mkdir routes && cd routes && touch api.js
```
![Routes](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fab86c7d-f8bc-48cb-a30a-24fe4294fa2a)

**2. Open the file with the command below**
```
vim api.js
```
```
const express = require('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

});

module.exports = router;
```

## Models
A model is at the heart of JavaScript based applications and it is what makes it interactive.

Models was used to define the database schema. This is important in order be able to define the fields stored in each Mongodb document.

In essence, the schema is a blueprint of how the database is constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties. To create a schema and a model, mongoose was installed, which is a Node.js package that makes working with mongodb easier.

**1. Change the directory back to Todo folder and install mongoose**
```
npm install mongoose
```
![Install mongoose](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8a077275-fa7a-47f4-8358-21c61e6617e9)

**2. Create a new folder models, switch to models directory, create a file todo.js inside models. Open the file**
```
mkdir models && cd models && touch todo.js
```
![Vim todo](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3b331e55-7653-4168-808b-70d778d5de5e)

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

// Create schema for todo
const TodoSchema = new Schema({
  action: {
    type: String,
    required: [true, 'The todo text field is required']
  }
});

// Create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
![paste script](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c9d80a50-c07f-4b1e-913b-68a631375c8d)

The routes was updated from the file api.js in the ‘routes’ directory to make use of the new model.

**3. In Routes directory, open api.js and delete the code inside with :%d.**
```
vim api.js
```
Paste the new code below
```
const express = require('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {
  // This will return all the data, exposing only the id and action field to the client
  Todo.find({}, 'action')
    .then(data => res.json(data))
    .catch(next);
});

router.post('/todos', (req, res, next) => {
  if (req.body.action) {
    Todo.create(req.body)
      .then(data => res.json(data))
      .catch(next);
  } else {
    res.json({
      error: "The input field is empty"
    });
  }
});

router.delete('/todos/:id', (req, res, next) => {
  Todo.findOneAndDelete({"_id": req.params.id})
    .then(data => res.json(data))
    .catch(next);
});

module.exports = router;
```
![vimjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a6c6ab1e-bfd7-4dc7-a737-58c9c6bccd43)

## MongoDB Database
mLab provides MongoDB database as a service solution (DBaaS). MongoDB has two cloud database management system components: mLab and Atlas, Both were formerly cloud databases managed by MongoDB (MongoDB acquired mLab in 2018, with certain differences). In November, MongoDB merged the two cloud databases and as such, mLab.com redirects to the MongoDB Atlas website.

**1. Create a MongoDB database and collection inside mLab**

MongoDB Cluster Overview

![MongoDB](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d9c53cd5-f856-4d5f-8168-a644515718d9)
