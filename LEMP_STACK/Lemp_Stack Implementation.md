## WEB STACK IMPLEMENTATION (LEMP STACK) 
## Introduction:
LEMP is an acronym for Linux, Nginx, MySQL, and PHP, which are four open-source technologies used to develop web and mobile applications. The LEMP stack is popular for its high performance, which comes from Nginx's ability to handle large amounts of traffic.
## Step 0: Preparing Prerequisites
**1.** EC2 instance of t.2 micro type and Ubuntu 24.04 LTS (HVM) was launched in the us-east-1 region using the AWS console.
![Lemp server created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d3790328-6ac7-4e5f-bd38-29cb4d834ab8)
**2.** Created SSH Key pair named "my-ec2-key.pem" to access the instance on the port.
**3** The security group was configured in the following inbound rules:

- Allow traffic on Port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on Port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on Port 22 (SSH) with source from any IP address. This is opened by default.

  ![Security Group Config](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/58ca9a76-3814-4157-b4d3-2af1b28d210b)

