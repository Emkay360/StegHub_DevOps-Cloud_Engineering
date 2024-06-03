## Web Solution With WordPress: Implementing a Basic Web Solution Using WordPress on Red Hat Linux
### Overview
This project involves setting up storage infrastructure on two Linux servers and implementing a basic web solution using WordPress. It consists of two main parts:

1. __Configure Storage Subsystem:__ For web and database servers based on Red Hat Linux OS.

2. __Install WordPress and Connect it to a Remote MySQL Database Server:__ Deploy a web solution using a three-tier architecture.
   
#### Three-tier Architecture Setup:

1. __Client:__ A laptop.
2. __Web Server:__ An EC2 instance where WordPress will be installed.
3. __Database Server:__ An EC2 instance for the MySQL database.

### Part 1: Configure Storage Subsystem for Web and Database Servers
#### Step 1: Prepare the Web Server

- __Launch an EC2 Instance:__

- Create an EC2 instance that will serve as the web server.
- Create three 10GiB volumes in the same Availability Zone (AZ) as the web server.
- Attach the volumes to the web server.

![Web Server EC2 created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fdef6de6-3905-404a-90e2-3de4695530f8)
![Security group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/5a44408c-2417-4aa3-a465-090d4f37c47a)

