![Create table](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b31f09e2-7abc-46fa-a66c-613a9222b364)## Task - Implement a Client-Server Architecture using MySQL Database Management System (DBMS)
### The following instructions were followed to implement the above task:
#### Step 1. Create and configure two Linux-based virtual servers (EC2 instance in AWS)
- ```mysql server```
- ```mysql client```
The ```client``` is the application that requests services from the server, such as retrieving or storing data, performing calculations, or executing commands.

The ```server``` is the application that provides services to the client, such as processing requests, sending responses, or completing actions.

__1.__ Two EC2 Instances of t2.micro type and Ubuntu 24.04 LTS (HVM) were launched in the US-East-1 region using the AWS console.

![My client Server](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/18719b9d-632e-43c4-ad16-9f00161205f7)

The security group inbound rule for both instances was configured with the default SSH on port 22 with source from anywhere.

__mysql Server__
![mysql security group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8e903a29-9cf8-429d-9818-194accc478e3)

__mysql Client__
![mysql client sec group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d99e0512-1945-4d19-aff2-53b5d0164745)

## Step 2 - On ```mysql server``` Linux Server, install MySQL Server software
__1.__ The private ssh key permission was changed for the private key file and then used to connect to the instance by running
```
ssh -i my-pair-key.pem ubuntu@54.226.238.114
```
![EC2 connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/24dbd731-6480-425e-ae82-384bb7a7686b)

__2.__ Update and upgrade Ubuntu

![update and upgrade](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/0ef9f461-07c5-4873-9097-4d281c5c68b5)

__3.__ Install MySQL Server software
```
sudo apt install mysql-server -y
```
![mysql for server](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/99167c3e-72db-4a20-8c9f-6bbed39a5c63)

__4.__ Enable mysql server
```
sudo systmctl enable mysql
```
![enable mysql](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e1f6665c-cb8d-4328-b146-0b467fc13f33)

## Step 3 - On mysql client Linux Server install MySQL Client software.
__1.__ Connect to the instance
```
chmod 400 my-pairs-key.pem
```
```
ssh -i my-pairs-key.pem ubuntu@18.212.250.8
```
![chmod for client](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/09da5e2a-a0ff-4258-8560-ef3143100b3c)

__2.__ Update and upgrade Ubuntu
```
sudo apt update && sudo apt upgrade
```
![update and upgrade 2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d2752767-de2e-4157-bc2d-e4b4fbfd1429)

__3.__ Install MySQL Client software
```
sudo apt install mysql-client -y
```
![install mysql client](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/ad4b5671-75a1-4175-95dd-103f5ca4d227)

## Step 4 - Use ```mysql server's``` local IP address to connect from ```mysql client```.
By default, both EC2 virtual servers are in the same local virtual network, so they can communicate using local IP addresses. Use ```mysql server's``` local IP address to connect from ```mysql client```. MySQL server uses ```TCP port 3306``` by default so it has to be opened by creating a new entry in inbound rules in mysql server Security Groups. For extra security, access to mysql server by all IP addresses was not allowed, only the specific local IP address of mysql client was allowed.

![mysql server security group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8cd09cbe-3cee-4c7b-a2d4-9af0831b41b9)

## Step 5 - Configure MySQL server to allow connections from remote hosts.
__Before the configuration stated above, the following were implemented:__

__1.__ The security script of MySQL was run on mysql server by running the command:
```
sudo mysql_secure_installation
```
![msql secure installation](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/266b3c74-2ddf-4b7d-b3ae-7ceadefff527)

__2.__ Access MySQL shell
```
sudo mysql
```
![sudo mysql](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f93d38f6-d1f2-4ad1-bde6-586df321697d)

__3.__ On ```mysql server```, create a user named ```client``` and a database named ```test_db```.
```
CREATE USER 'client'@'%' IDENTIFIED WITH mysql_native_password BY 'User123$';

CREATE DATABASE test_db;

GRANT ALL ON test_db.* TO 'client'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```
![creating user](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fd9a8771-6491-4701-bca6-f7d8b67866d7)

__4.__ Now, configure MySQL server to allow connections from remote hosts.
```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
Locate ```bind-address = 127.0.0.1```

Replace ```127.0.0.1``` with ```0.0.0.0```

![myswl databse](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/954a7963-a98c-4f25-89a8-e982a6283a62)

## Step 6 - From ```mysql client``` Linxus Sever, connect remotely to ```mysql server``` Database Engine without SSH. The mysql utility must be used to perform this action.
```
sudo mysql -u admin -p -h 172.31.24.181
```
![logged in](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/dd53aa54-17c6-46ae-8f6f-a8b993e4f29e)
## Step 7 - Check that the connection to the remote MySQL server is successful and can perform SQL queries.
```
show databases;
```
![Show databases](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/86edac27-4936-4959-8df0-75b90dc10b9d)

__Create a table, insert rows into a table, and select from the table__
```
CREATE TABLE test_db.test_table (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);

INSERT INTO test_db.test_table (content) VALUES ("My first choice of food is rice");

INSERT INTO test_db.test_table (content) VALUES ("My second choice of movie is Avengers");

SELECT * FROM test_db.test_table;
```
![Create table](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/15fb0640-0685-44a3-879f-df1549d7cb5e)

At this point, this project is successfully complete. This deployment is a fully functional MySQL Client-Server set up.



