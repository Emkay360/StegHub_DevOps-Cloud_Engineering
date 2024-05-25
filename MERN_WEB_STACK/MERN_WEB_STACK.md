## MERN STACK IMPLEMENTATION IN AWS 
### Introduction:
The MERN stack is a collection of technologies that help developers build robust and scalable web applications using JavaScript. The acronym “MERN” stands for MongoDB, Express, React, and Node.js, with each component playing a role in the development process. MongoDB is a document-oriented database that can efficiently store data in JSON format.

**This guide shows a comprehensive overview of setting up and utilizing each component of the MERN stack in creating and developing robust web applications**

## Step 0: Prerequisites

**1.** EC2 Instance of t3.small type and Ubuntu 24.04 LTS (HVM) was launched in the US-east-1 region using the AWS console. The choice of the instance type was based on the following:

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
![vimjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c4836b8d-b9fa-41cc-b4ed-7912304998df)

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

AWS cloud provider, in region N. Virginia (us-east-1) was selected.

![Network access](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2c52bf88-0558-4c63-a7f1-801fd3988b5b)

Access from anywhere to the MongoDB database was allowed (Not secure but it is ideal for testing).

![Network access](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/73526886-5d47-48e9-b4df-47aff86c8d21)

A database named todo_db and collections named todos was created.

![Todo](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fe062053-d0c5-4b9f-96fc-2678e853238c)

__2. Create a file in your Todo directory and name it .env, open the file__

```
touch .env && vim .env
```
Add the connection string below to access the database
```
DB = ‘mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority’
```
![mongodb](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a3da8849-f380-4423-9728-48c325b79106)

__3. Update the index.js to reflect the use of .env so that Node.js can connect to the database.__
```
vim index.js
```
Delete existing content in the file, and update it with the entire code below:
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

// Connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log(`Database connected successfully`))
  .catch(err => console.log(err));

// Since mongoose promise is deprecated, we override it with Node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
  console.log(err);
  next();
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```

![vim indexjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9c536fc3-9ccf-4eb8-99af-ed6ba8c19c94)

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

__4. Start your server using the command__
```
node index.js
```
![DB connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7a3915f3-82c8-4c19-bee6-652d0d068860)

## Testing Backend Code without Frontend using RESTful API
Postman was used to test the backend code. The endpoints were tested. JSON was sent back with the necessary fields for the endpoints that require a body since it’s what was set up in the code.

__1. Open Postman and Set the header__
```
http://54.91.117.155:5000/api/todos
```
__Creating ```GET``` request to the API__

![postman connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e440e84f-ddb4-403b-8edb-6c926f1e675d)

__Creating ```POST``` request to the API__

![postman POST](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a0153c38-b3dd-495f-9faf-38c9f37a01ef)

![Multiple testing](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1d83095b-8a40-4560-8874-4a02dd006f06)

__Create a ```DELETE``` request to the API__

![Delete post](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/496ff434-629c-4065-bddc-462c1a7864a9)

__Hint:__ To delete a task, make sure to add its ID as part of ```DELETE``` request
```
54.91.117.155:5000/api/todos/6650ae2ff5f474eaa9b54f04
```
## Step 2: Frontend Creation
It is time to create a user interface for a Web client (browser) to interact with the application via the API

- In the same root directory as your backend code, which is the Todo directory, run:
```
npx create-react-app client
```
![Create react app](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/944402ba-766e-4d82-96a2-48e819a282b3)

This will create a folder in the ```Todo```directory called ```client``` , where all the react code will be added

## Running a React App
Before testing the react app, the following dependencies need to be installed in the project root directory.

- Install __concurrently__. It is used to run more than one command simultaneously from the same terminal window.
```
npm install concurrently --save-dev
```
![concurrent install](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b77bff09-c95b-4125-8106-4756eaaef3df)

- Install __nodemon__. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
```
npm install nodemon --save-dev
```
![install nodemon](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/edba87c0-1aaa-44c1-a312-ee93d87745a2)

Nodemon is used to run the server and monitor it as well. If there is any change in the server code, Nodemon will restart it automatically with the new changes.
- Next, open your ```package.json``` file in the Todo directory of the app project, and change the highlighted part of the following code:
```
"scripts": {
  "start": "node index.js",
  "start-watch": "nodemon index.js",
  "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
}
```
![concurrenlty script](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/dcbe8144-eacc-4ac6-9c84-37a72b65a6c0)

## Configure Proxy In ```package.json```
- Change the directory to “client”
```
cd client
```
- Open the ```package.jason``` file
```
vim package.json
```
![proxy](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b6f715c8-dc6b-4c76-8fbd-c7bbed7a57d3)

The app opened and started running on localhost:3000

__Note:__ To access the application from the internet, TCP port 3000 has been opened on EC2.

## Creating React Components
One of the advantages of React is that it makes use of components, which are reusable and also makes code modular. The Todo app has two stateful components and one stateless component. From Todo directory, run:
```
cd client
```
Move to the ```src``` directory
```
cd src
```
Create another file under the ```src``` and call it ```components```
```
mkdir components
```
Move into the ```components```
```
cd components
```
Inside the ‘components’ directory create three files “Input.js”, “ListTodo.js” and “Todo.js”.
```
touch Input.js ListTodo.js Todo.js
```
![src](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9da753bb-dced-435a-a7a3-fe0a21a93057)

- Open ```Input.js```
```
vim input.js
```
Paste the following command:
```
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {
  state = {
    action: ""
  }

  handleChange = (event) => {
    this.setState({ action: event.target.value });
  }

  addTodo = () => {
    const task = { action: this.state.action };

    if (task.action && task.action.length > 0) {
      axios.post('/api/todos', task)
        .then(res => {
          if (res.data) {
            this.props.getTodos();
            this.setState({ action: "" });
          }
        })
        .catch(err => console.log(err));
    } else {
      console.log('Input field required');
    }
  }

  render() {
    let { action } = this.state;
    return (
      <div>
        <input type="text" onChange={this.handleChange} value={action} />
        <button onClick={this.addTodo}>add todo</button>
      </div>
    );
  }
}

export default Input;
```
![Inputjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e44e9ca1-cde9-40af-9d34-b0ee80fbb603)

To make use Axios, a Promise-based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run yarn add axios or npm install axios.

- Move to the client folder
```
cd ..
```
- Move to the clients folder
```
cd ..
```
- Install Axios
```
npm install axios
```
- Go to the ```components``` directory
```
cd src/components
```
- After that open your ```ListTodo.js```
```
vim ListTodo.js
```
Past the following command:
```
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {
  return (
    <ul>
      {
        todos && todos.length > 0 ? (
          todos.map(todo => {
            return (
              <li key={todo._id} onClick={() => deleteTodo(todo._id)}>
                {todo.action}
              </li>
            );
          })
        ) : (
          <li>No todo(s) left</li>
        )
      }
    </ul>
  );
}

export default ListTodo;
```
![Import react](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/246f55d1-ffa4-42a2-85de-0211c0819e88)

Then in the Todo.js write the following command:
```
vim Todo.js
```
```
import React, { Component } from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {
  state = {
    todos: []
  }

  componentDidMount() {
    this.getTodos();
  }

  getTodos = () => {
    axios.get('/api/todos')
      .then(res => {
        if (res.data) {
          this.setState({
            todos: res.data
          });
        }
      })
      .catch(err => console.log(err));
  }

  deleteTodo = (id) => {
    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if (res.data) {
          this.getTodos();
        }
      })
      .catch(err => console.log(err));
  }

  render() {
    let { todos } = this.state;
    return (
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos} />
        <ListTodo todos={todos} deleteTodo={this.deleteTodo} />
      </div>
    );
  }
}

export default Todo;
```
![Todojs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/39298d5b-6ba4-4db9-a7c4-553834ea4efb)

- We need to make a little adjustment to our react code. Delete the logo and adjust our App.js to look like this
Move to the src folder
```
cd ..
```
Ensure to be in the src folder and run:
```
vim App.js
```
- Copy and paste the following code
```
import React from 'react';
import Todo from './components/Todo';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <Todo />
    </div>
  );
}

export default App;
```
![appjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c6c84fb4-9747-4af8-87a8-df0767ebbe44)

- In the ```src``` directory, open the ```App.css```
```
vim App.css
```
Paste the following code into it
```
.App {
  text-align: center;
  font-size: calc(10px + 2vmin);
  width: 60%;
  margin-left: auto;
  margin-right: auto;
}

input {
  height: 40px;
  width: 50%;
  border: none;
  border-bottom: 2px #101113 solid;
  background: none;
  font-size: 1.5rem;
  color: #787a80;
}

input:focus {
  outline: none;
}

button {
  width: 25%;
  height: 45px;
  border: none;
  margin-left: 10px;
  font-size: 25px;
  background: #101113;
  border-radius: 5px;
  color: #787a80;
  cursor: pointer;
}

button:focus {
  outline: none;
}

ul {
  list-style: none;
  text-align: left;
  padding: 15px;
  background: #171a1f;
  border-radius: 5px;
}

li {
  padding: 15px;
  font-size: 1.5rem;
  margin-bottom: 15px;
  background: #282c34;
  border-radius: 5px;
  overflow-wrap: break-word;
  cursor: pointer;
}

@media only screen and (min-width: 300px) {
  .App {
    width: 80%;
  }

  input {
    width: 100%;
  }

  button {
    width: 100%;
    margin-top: 15px;
    margin-left: 0;
  }
}

@media only screen and (min-width: 640px) {
  .App {
    width: 60%;
  }

  input {
    width: 50%;
  }

  button {
    width: 30%;
    margin-left: 10px;
    margin-top: 0;
  }
}
```
![appjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/75a8d58a-f062-46d1-8ca1-97bfd699bc1d)

- In the ```src``` directory, open the ```index.css```
```
vim index.css
```
```
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  box-sizing: border-box;
  background-color: #282c34;
  color: #787a80;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New", monospace;
}
```
![indexcss](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7ff4703a-395b-4fe9-9674-2a70b70ced44)

- Go to the Todo directory
```
cd ../..
```
Run:
```
npm run dev
```
![npm dev](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8c19d58c-cdc6-47e0-9e5f-f43a34e99c2e)

At this point, the To-Do app is ready and fully functional with the functionality discussed earlier: Creating a task, deleting a task, and viewing all the tasks.

