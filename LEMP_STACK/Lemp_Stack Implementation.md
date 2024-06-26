## WEB STACK IMPLEMENTATION (LEMP STACK) 
## Introduction:
LEMP is an acronym for Linux, Nginx, MySQL, and PHP, which are four open-source technologies used to develop web and mobile applications. The LEMP stack is popular for its high performance, which comes from Nginx's ability to handle large amounts of traffic.
## Step 0: Preparing Prerequisites
**1.** EC2 instance of t.2 micro type and Ubuntu 24.04 LTS (HVM) was launched in the us-east-1 region using the AWS console.

![Lemp server created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d3790328-6ac7-4e5f-bd38-29cb4d834ab8)

**2.** Created SSH Key pair named "my-ec2-key.pem" to access the instance on the port.
**3** The security group was configured with the following inbound rules:

- Allow traffic on Port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on Port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on Port 22 (SSH) with source from any IP address. This is opened by default.

  ![Security Group Config](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/58ca9a76-3814-4157-b4d3-2af1b28d210b)

**4** The default VPC and Subnet were used for the network configuration. 

![VPC](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/dc2cf623-4f8d-434b-a070-1e836306a156)
![Subnet](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/4a382e40-b3cc-42fb-bbf0-96c49ba55e3f)

**5** The private SSH key pair that got downloaded was located and was used to connect to the instance running.
```
ssh -i "my-ec2-key.pem" ubuntu@18.209.18.61
```
where **username=ubuntu** and **public ip address=54.196.130.53**

![Connecting EC2 with Bash](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/5f0b7506-40bd-4078-8a6f-234adca0ecc7)

## Step 1: Install the nginx Web Server
**1. Update and Upgrade**
```
sudo apt update
sudo apt upgrade -y
```
![Sudo apt update](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c1b474f0-90b6-436c-98c3-0f8280ea26d6)
![sudo apt upgrade -y](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f8717414-7dcb-4aa9-8532-3d1200ad05f7)

**2. Install nginx**
```
sudo apt install nginx
```
![Install nginx](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/466fbb17-1fd7-40b8-9439-46ffa377b6e9)

**3. Verify that nginx is working**
```
sudo systemctl status nginx
```
![nginx status](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/530fc4f2-3b2d-4101-a8d6-064242eeb8ea)

**4. Access nginx locally on the Ubuntu Shell**
```
curl http://localhost:80
```
![checking nginx locally](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7de08141-24ce-4997-b4c9-0bd89d3b1c4f)

**5. Test with the public ip address if the nginx server can respond to requests from the internet using the URL on the web browser**
```
http://54.196.130.53
```
![nginx page](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/ebc4ac81-0ae8-4425-b9a3-56109819eb41)

## Step 2: Install MySQL

**1. Installed MySQL Server**
```
sudo apt install mysql-server
```
Press y when prompted.

![Installing mysql server (lemp)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/37c23882-59a8-4fb9-90e1-67bfd8b60024)

**2. Log into the MySQL console**
```
sudo mysql
```

![mysql login](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/673b5d27-ed39-4020-9d6c-1973a8a6819a)

This connects to the MySQL server as the administrative database user root inferred by the use of sudo when running the command.

**3 Set a password for the root user using mysql_native_password as a default authentification method**

Here is the password and it was defined as 'Emkay360'
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Emkay360';
```
![mysql password](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2996ae36-4e3c-43bb-b28e-590b9d841e44)

Then exit MySQL
```
mysql> exit
```
**4 Run the interactive script**
```
sudo mysql_secure_installation
```
![Create a new password](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/4d83a98d-a0ef-4080-aa53-cddd80b74c32)

**After changing the root user password, log into MySQL**
```
sudo mysql -p
```
![Login into mysql](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/5e52b535-c0e2-4766-aabd-ad97661971a6)

Then exit MySQL shell
```
exit
```
## Step 3: Install PHP
**1 Install PHP**
Install php-fpm (PHP fastCGI process manager) and tell nginx to pass PHP requests to this software for processing. Also, install php-mysql, a php module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.

The following were installed:

- php-fpm (PHP fastCGI process manager)
- php-mysql
```
sudo apt install php-fpm php-mysql
```
![Installing PHP](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b28e4441-ae22-424e-9300-633f9e3a2f3a)

## Step 4: Configuring Nginx to Use PHP Processor
**1 Create a root web directory for your_domain**
```
sudo mkdir /var/www/projectLEMP
```
**2 Assign ownership of the directory with the $USER environment variable which will reference your current system user**
```
sudo chown -R $USER:$USER /var/www/projectLEMP
```
![User directory created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a90e1105-2866-4075-95f0-3a067f3038a3)

**3. Create a new configuration file in Nginx’s “sites-available” directory.**
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
Paste in the following bare-bones configuration:
```
server {
  listen 80;
  server_name projectLEMP www.projectLEMP;
  root /var/www/projectLEMP;

  index index.html index.htm index.php;

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
  }

  location ~ /\.ht {
    deny all;
  }
}
```
![Bare bone configuration](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/44a5c5ed-f63b-466e-bcaa-4a248ad07f1a)

**Here’s what each directive and location blocks does:**

- listen - Defines what port nginx listens on. In this case, it will listen on port 80, the default port for HTTP.

- root - Defines the document root where the files served by this website are stored.

- index - Defines in which order Nginx will prioritize the index files for this website. It is a common practice to list index.html files with higher precedence than index.php files to allow for quickly setting up a maintenance landing page for PHP applications. You can adjust these settings to suit your application needs better.

- server_name - Defines which domain name and/or IP addresses the server block should respond for. Point this directive to your domain name or public IP address.

- location / - The first location block includes the try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate result, it will return a 404 error.

- location ~ .php$ - This location handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.

- location ~ /.ht - The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors.

**4 Activate the configuration by linking to the config file from Nginx's sites-enabled directory:**
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
![sites available](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c426b265-4f35-47b1-80fb-2e07e0d8467c)

This will tell Nginx to use this configuration when next it is reloaded.

**5 Test the configuration syntax error**
```
sudo nginx -t
```
![syntax okay](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2b8ec8fe-7f91-4960-8e20-3b203a0fa6c6)

**6 Disable the default Nginx host that is currently configured to listen on port 80**
```
sudo unlink /etc/nginx/sites-enabled/default
```
![Disable Nginx](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/474f7bdf-d44f-473f-b61b-d1f77e323d8b)

**7 Reload Nginx to apply changes**
```
sudo systemctl reload nginx
```
![Reload nginx](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fcf970a3-ac73-48c0-8291-d6eb9c45efa2)

**8 The new website is now active but the web root /var/www/projectLEMP is still empty. Create an index.html file in this location to test the expected virtual host work.**
```
sudo echo ‘Hello LEMP from hostname’ $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) ‘with public IP’ $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```
![Ocupy web root](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f81b8a2a-ddda-44eb-96a8-8d02829e60f1)

**9 Open the IP address**
```
http://34.236.154.135:80
```
![IP address opened](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d3a59b08-dd3b-41d3-967b-d0048d54cfb1)

**10 Open with public DNS (port is optional)**
```
http://ec2-34-236-154-135.compute-1.amazonaws.com
```
![Public DNS used to open site](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/406185f6-3985-40ff-ba42-a075ed21827f)

This file can be left in place as a temporary landing page for the application until an index.php file is set up to replace it. Once this is done, remove or rename the index.html file from the document root as it will take precedence over the index.php file by default.

The LEMP stack is now fully configured. At this point, the LEMP stack is completely installed and fully operational.

## Step 5: Testing PHP with Nginx
Test the LEMP stack to validate that Nginx can correctly hand .php files off to the PHP processor.

**1 Create a test PHP file in the document root. Open a new file called info.php within the document root.**
```
sudo nano /var/www/projectLEMP/info.php
```
There was a "502 bad Gateway" error

![502 error](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e12b3dd5-3674-484a-963d-25ca1aef9273)

**2 Reconfigured the projectLEMP file by creating a symbolic link**

- First, create a new project file using the link
```
sudo nano /etc/nginx/sites-available/projectLEMP
```
- Then copied and pasted the link below:
```
# /etc/nginx/sites-available/projectLEMP
     server {
     listen 80;
     server_name projectLEMP www.projectLEMP;
     root /var/www/projectLEMP;
     index index.html index.htm index.php;
     location / {
         try_files $uri $uri/ =404;
     }
     location ~ \.php$ {
         include snippets/fastcgi-php.conf;
         fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }
     location ~ /\.ht {
         deny all;
     }
 }
```
- Saved and created the symbolic link
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
- It failed to create the link

![Failed to create link as file exists](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/63f5e5e7-a80f-4f99-8e40-9228664c6b5a)

- Go to sites-available to delete default and projectLEMP
```
sudo rm projectLEMP
sudo rm default
```
![rm default and proctLEMP](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a01d9ade-d0ed-4619-b0fe-59ac008b87fb)

Still, there was an error linking and enabling sites

- Next, go to sites-enabled and delete projectLEMP
```
cd etc/nginx/sites-enabled
```
```
sudo rm projectLEMP
```
![sites enabled](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9a40fbaa-4161-4008-8891-e0a734f9dbf4)
 
- Lastly, link and enable projectLEMP
```
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
```
```
sudo nginx -t
```
syntax successful and ok

- Restarted nginx
```
sudo systmectl restart nginx
```
**3 Retested PHP with Nginx**

![PHP Nginx Ok](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f4e97dcb-01e6-4f9c-bfbe-7da946cbdd95)

After checking the relevant information about the server through this page, It’s best to remove the file created as it contains sensitive information about the PHP environment and the Ubuntu server. It can always be recreated if the information is needed later.

## Step 6: Retrieving Data from MySQL Database with PHP
## Create a new user with mysql_native_password authentication method to be able to connect to MySQL database from PHP.

Create a database named todo_database and a user named todo_user

**1 Connect to the MySQL console using the root account:**
```
sudo mysql -p
```
![Connected mysql](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/6b13200c-7a08-4579-97ab-311e97b9844b)

**2 Create a new database**
```
CREATE DATABASE todo_database;
```
![Created a new database](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/14493514-5591-44ce-b300-9a53009cff21)

**3 Create a new user and grant the user full privileges**
```
CREATE USER 'todo_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Muhammad$360';

GRANT ALL ON todo_database.* TO 'todo_user'@'%';
```
![News user granted full privilledges](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/23928ac2-3231-4956-bcd1-99f33a21f39b)

Exit MySQL
```
exit
```
**4 Login to MySQL console using the user's credentials**

```
mysql -u todo_user -p
```
```
SHOW DATABASES
```
![logged in with user credentials](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b19a6400-7cce-440c-bd27-4ba0b02f7623)

**5 Create a table named todo_list**
```
CREATE TABLE todo_database.todo_list (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);
```
**6 Insert a few rows of content in the test table**
```
INSERT INTO todo_database.todo_list (content) VALUES ("My first important item");

INSERT INTO todo_database.todo_list (content) VALUES ("My second important item");

INSERT INTO todo_database.todo_list (content) VALUES ("My third important item");

INSERT INTO todo_database.todo_list (content) VALUES ("and this one more thing");
```
![Created a table](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/df2742ec-27eb-45af-8ac3-7e670bc67a7e)

**7 Confirm if the data was successfully saved on the table**
```
SELECT * FROM todo_database.todo_list;
```
![Confirm table](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8d0c3d63-b9eb-4a2e-8828-ba1945ab4829)

Exit MySQL
```
exit
```
## Create a PHP script that will connect to MySQL and query the content.

**1 Create a new PHP file in the custom web root directory**
```
sudo nano /var/www/projectLEMP/todo_list.php
```
The PHP script connects to MySQL database and queries for the content of the todo_list table display the results in a list. If there’s a problem with the database connection, it will throw an exception.

Copy the content into the todo_list.php
```
<?php
$user = "todo_user";
$password = "Muhammad$360";
$database = "todo_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
?>
```
![php script](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/691499fb-90a8-4bdd-88d8-147b752a627c)

**2 Access this page on the browser by using the domain name or public IP address**
```
http://44.222.207.129/todo_list.php
```
![To do list](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f2c55e60-c3f3-4353-a489-3417697b3d51)

PHP environment is ready to connect and interact with the MySQL server.
