![image](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1e1bae3c-a836-4f93-9b6f-b06926e01a0c)## MEAN Stack Deployment to Ubuntu
### Introduction:
The MEAN stack is a JavaScript-based framework for developing scalable web applications. The term MEAN is an acronym for MongoDB, Express, Angular, and Node — the four key technologies that make up the layers of the technology stack.

- __MongoDB__: A NoSQL, object-oriented database designed for use with cloud applications
- __Express(.js)__: A web application framework for Node(.js) that supports interactions between the front end (e.g., the client side) and the database
- __Angular(.js)__: Often referred to as the “front end"; a client-side JavaScript framework used to create dynamic web applications to work with interactive user interfaces
- __Node(.js)__: The premier JavaScript web server used to build scalable network applications

## Step 0: Preparing Prerequisites
__1.__ EC2 Instance of t3.small type and Ubuntu 22.04 LTS (HVM) was launched in the US-East-1 region using the AWS console.

![Mean server](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/0d8f06f2-ae43-4109-906f-1cd6b011facf)

__2.__ Attached SSH key named my-e2-key to access the instance on port 22

__3.__ The security group was configured with the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.

- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.

- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default.

- Allow traffic on port 3300 (Custom TCP) with source from anywhere.

![Security group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c37596d9-601f-4729-a555-db038fa7d6c4)

__4. Connected to the instance__
```
ssh i- my-e2-key.pem ubuntu@54.87.10.79
```
![Ubuntu connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/73d10322-19ca-4633-ad58-d252685bc01e)

Where username=ubuntu and public IPaddress=54.87.10.79

## Step 1 - Install Nodejs
Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this tutorial to set up the Express routes and AngularJS controllers.

__1. Update and Upgrade Ubuntu__
```
sudo apt update && sudo apt upgrade -y
```
![ubuntu update](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/33e5d4fe-384f-4b5d-bd6b-73d91949084d)

__2. Add certificates__
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
```
![update cert](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/54f61763-4ec2-418f-a120-e2fa6709b134)
```
curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```
![adding cert](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e37fe7f3-bba2-4d87-9e22-fef045ceee35)

__3. Install Node.js__
```
sudo apt install -y nodejs
```
![Install nodejs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/77710aba-e9ab-4457-8b59-1d850d9f7d9d)

## Step: Install MongoDB
For this application, Book records were added to MongoDB that contain the book name, ISBN, author, and number of pages.

__1. Download the MongoDB public GPG key__
```
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg --dearmor -o /usr/share/keyrings/mongodb-archive-keyring.gpg
```
__2. Add the MongoDB repository__
```
echo "deb [ signed-by=/usr/share/keyrings/mongodb-archive-keyring.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```
![Added MongoDB](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d621da24-410f-4c57-8a77-df63833bce92)

__3. Update the package database and install MongoDB__
```
sudo apt-get update
```
![sudo apt get](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/be827ee1-56a3-44ff-991c-2157aefd2c18)
```
sudo apt-get install -y mongodb-org
```
![Install MDB](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e46fe94d-5c22-4c9b-9f6b-51be91e3fad9)

__4. Start and enable MongoDB__
```
sudo systemctl start mongod
```
```
sudo systemctl enable mongod
```
```
sudo systemctl status mongod
```
__5. Install body-parser package__
```
sudo npm install body-parser
```
![body parser](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c40a2e99-207f-49ae-b3b7-224bf65ca074)

__6. Create the project root folder named ‘Books’__
```
mkdir Books && cd Books
```
Initialize the root folder
```
npm init
```
![book folder created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3b72f39b-0dba-4557-bbeb-8fdd2830cc45)
![added](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1849411f-6dcc-4f4c-b716-956e9ca27057)

__Add file named server.js to Books folder__
```
vim server.js
```
Copy and paste the web server code below into the server.js file.
```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose'); // Make sure mongoose is installed and required
const path = require('path'); // To handle static file serving
const app = express();

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/test', { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected'))
  .catch(err => console.error('MongoDB connection error:', err));

// Middleware
app.use(bodyParser.json());
app.use(express.static(path.join(__dirname, 'public')));

// Routes
require('./apps/routes')(app);

// Start the server
app.set('port', 3300);
app.listen(app.get('port'), () => {
  console.log('Server up: http://localhost:' + app.get('port'));
});
```
## Step 3 - Install Express and set up routes to the server
Express was used to pass book information to and from our MongoDB database. Mongoose package provides a straightforward schema-based solution to model the application data. Mongoose was used to establish a schema for the database to store data of the book register.

__1. Install express and mongoose__
```
sudo npm install express mongoose
```
![express mongoose](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fb00bd7b-c5f3-4955-89df-3d2ba9702979)

__2. In the Books folder, create a folder named ‘apps’__
```
mkdir apps && cd apps
```
__In apps, create a file named routes.js__
```
vim routes.js
```
Copy and paste the code below into routes.js
```
const Book = require('./models/book');
const path = require('path');

module.exports = function(app) {
  // Get all books
  app.get('/book', async (req, res) => {
    try {
      const books = await Book.find({});
      res.json(books);
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Add a new book
  app.post('/book', async (req, res) => {
    try {
      const book = new Book({
        name: req.body.name,
        isbn: req.body.isbn,
        author: req.body.author,
        pages: req.body.pages
      });
      const result = await book.save();
      res.json({
        message: "Successfully added book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Update a book
  app.put('/book/:isbn', async (req, res) => {
    try {
      const updatedBook = await Book.findOneAndUpdate(
        { isbn: req.params.isbn },
        req.body,
        { new: true }
      );
      if (!updatedBook) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully updated the book",
        book: updatedBook
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Delete a book
  app.delete('/book/:isbn', async (req, res) => {
    try {
      const result = await Book.findOneAndRemove({ isbn: req.params.isbn });
      if (!result) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Serve static files
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};
```
![routejs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e1cd7a9d-b647-4006-aa97-2b197dbfcae9)

__3. In the ‘apps’ folder, create a folder named models__
```
mkdir models && cd models
```
__In models, create a file named book.js__
```
vim book.js
```
Copy and paste the code below into book.js
```
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  name: { type: String, required: true },
  isbn: { type: String, required: true, unique: true },
  author: { type: String, required: true },
  pages: { type: Number, required: true }
});

module.exports = mongoose.model('Book', bookSchema);
```
![vim book](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/09a145f9-90b9-49dd-9e61-69cd23827f80)
![book file added](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/678caf0b-3246-4bff-8959-60c37ca55cfb)

## Step 4 - Access the routes with AngularJS
In this project, AngularJS was used to connect the web page with Express and perform actions on the book register.

__1. Change the directory back to ‘Books’ and create a folder named ‘public’__
```
cd ../..

mkdir public && cd public
```
Add a file named script.js into public folder
```
vim script.js
```
Copy and paste the code below (controller configuration defined) into the script.js file.
```
var app = angular.module('myApp', []);

app.controller('myCtrl', function($scope, $http) {
  // Get all books
  function getAllBooks() {
    $http({
      method: 'GET',
      url: '/book'
    }).then(function successCallback(response) {
      $scope.books = response.data;
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  }

  // Initial load of books
  getAllBooks();

  // Add a new book
  $scope.add_book = function() {
    var body = {
      name: $scope.Name,
      isbn: $scope.Isbn,
      author: $scope.Author,
      pages: $scope.Pages
    };
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
      // Clear the input fields
      $scope.Name = '';
      $scope.Isbn = '';
      $scope.Author = '';
      $scope.Pages = '';
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Update a book
  $scope.update_book = function(book) {
    var body = {
      name: book.name,
      isbn: book.isbn,
      author: book.author,
      pages: book.pages
    };
    $http({
      method: 'PUT',
      url: '/book/' + book.isbn,
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Delete a book
  $scope.delete_book = function(isbn) {
    $http({
      method: 'DELETE',
      url: '/book/' + isbn
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };
});
```
![scriptjs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8fa2233e-9277-43ae-a596-ca6178952eb0)

__2. In ‘public’ folder, create a file named index.html__
```
vim index.html
```
Copy and paste the code below into index.html file
```
<!DOCTYPE html>
<html ng-app="myApp" ng-controller="myCtrl">
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
  <script src="script.js"></script>
  <style>
    /* Add your custom CSS styles here */
  </style>
</head>
<body>
  <div>
    <table>
      <tr>
        <td>Name:</td>
        <td><input type="text" ng-model="Name"></td>
      </tr>
      <tr>
        <td>Isbn:</td>
        <td><input type="text" ng-model="Isbn"></td>
      </tr>
      <tr>
        <td>Author:</td>
        <td><input type="text" ng-model="Author"></td>
      </tr>
      <tr>
        <td>Pages:</td>
        <td><input type="number" ng-model="Pages"></td>
      </tr>
    </table>
    <button ng-click="add_book()">Add</button>
    <div ng-if="successMessage">{{ successMessage }}</div>
    <div ng-if="errorMessage">{{ errorMessage }}</div>
  </div>
  <hr>
  <div>
    <table>
      <tr>
        <th>Name</th>
        <th>Isbn</th>
        <th>Author</th>
        <th>Page</th>
        <th>Action</th>
      </tr>
      <tr ng-repeat="book in books">
        <td>{{ book.name }}</td>
        <td>{{ book.isbn }}</td>
        <td>{{ book.author }}</td>
        <td>{{ book.pages }}</td>
        <td><button ng-click="del_book(book)">Delete</button></td>
      </tr>
    </table>
  </div>
</body>
</html>
```
![indexhtml](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8ed6b19f-ce85-43f7-af78-678d1c6e6105)

Start the server by running the command:
```
node server.js
```
![MongoDB connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/db90649a-35dd-4eea-b3e1-4ec722c275c3)

The server is now up and running, Connection to it is via port 3300. A separate Putty or SSH console to test what curl command returns locally can be launched.

The Book Register web application can now be accessed from the internet with a browser using the Public IP address or Public DNS name.

![Page connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/86e3c467-8764-4371-b488-6a1a241e6efc)

Add more books to the register

![Multiple ISBN](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/572161a8-ca1f-4963-b589-6b11689e12e4)

## Conclusion
The MEAN stack—comprising MongoDB, Express.js, AngularJS (or Angular), and Node.js—provides a powerful and cohesive set of technologies for building modern web applications.

Together, these technologies allow developers to use JavaScript throughout the entire development process, from front-end to back-end, promoting a unified and streamlined development workflow.
