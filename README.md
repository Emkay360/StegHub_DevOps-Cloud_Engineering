# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS 103
## Introduction:
LAMP is an open-sourced web development used by developers. It consists of 4 components: Linux (Operating system), Apache (web server software that delivers content through the internet), MySQL (An open source relational database for storing applications), PHP (Sometimes Perl or Python, a programming language that helps Apache in creating dynamic web pages). 
# Step 0: Prerequisites
**1.** EC2 instance of t2.micro type and Ubuntu 24.04 LTS (HVM) was launched in us-east-1 region using the AWS console  

![Screenshot (138)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/549148bd-6768-4e22-b09c-75e5108770d8)
![Screenshot (139)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7805438c-c68e-40a5-ad7d-0bcb283669e3)  

**2** Created a new SSH key pair to access the instance on port 22

**3** The security group was configured using the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default
  
![Screenshot (140)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2ec68129-cda8-4f29-a419-fc75b5c6059e)

**4** For the network configuration, the default VPS subnet was used.

![Screenshot (141)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2bf58110-92d1-4e02-98f1-1283f9adeaad)

**5** The private ssh key downloaded was used to connect to the instance.

```
chmod 400 my-private-key.pem
```
```
ssh -i my-private-key.pem @ubuntu3.93.185.64.compute-1.amazonaws.com
```

![Install apache2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1637ad7a-bb54-4f1f-802b-87b9b5075c1e)

## Step 1 - Install Apache2 and update its Firewall
```
sudo apt update
```
```
sudo apt install apache2
```
![Screenshot (153)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b72ef6ac-5cc8-495e-a89a-f0c75341b197)





**2** **Verify and run Apache package**
```
sudo systemctl enable
sudo systemctl status apache2
```
![Screenshot (149)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/75eb38a7-6bd8-43ae-97b6-219660749d4a)

