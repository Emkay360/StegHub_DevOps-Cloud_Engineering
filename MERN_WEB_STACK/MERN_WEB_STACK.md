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


