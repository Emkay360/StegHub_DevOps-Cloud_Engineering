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

**3 Set a password for the root user using mysql_native_password as a default authentification method**
Here is the password and it was defined as 'Emkay360'
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Emkay360';
```
![mysql password](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2996ae36-4e3c-43bb-b28e-590b9d841e44)

