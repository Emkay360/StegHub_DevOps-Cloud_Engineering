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

![Screenshot (153)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/6aea6fe2-ff07-49f7-aa42-5ca240f9f22e)


## Step 1 - Install Apache2 and update its Firewall
```
sudo apt update
```
```
sudo apt install apache2
```
![Install apache2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1637ad7a-bb54-4f1f-802b-87b9b5075c1e)

**2** **Enable and verify Apache2**
If it is green, then it is running well
```
sudo systemctl enable
sudo systemctl status apache2
```
![apache2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8efaf5ae-e567-4463-84b9-0b29e8fa0e36)

**3** **Checking if the server is running and can be accessed locally through the Ubuntu Shell**
```
curl http://localhost:80
```
![Local host](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1ed7352d-4489-45f3-80a0-16845c5703b0)

**4** **Testing how our Apache server can respond from the internet**
```
http://3.93.185.64:80
```
![Screenshot (150)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/70cb8a0a-c9ef-473b-b959-371805341220)

**Used another way to retrieve the public address other than the AWS console**
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
The result return with a 404 not found
![404 not found](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/cfbf49e3-fd40-43bf-9ed5-3e4a614c945c)

I troubleshoot the error following the modifying the instance
- Action: Instance Settings > Modify instance Meta Data
- Change the settings in **IMDSv2** from **Required** to **Optional**
![Screenshot (154)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/d08ac0bc-96cf-4949-bfb2-6749b7b57ef6)
```
curl -s http://169.254.169.254/latest/meta-data/public-ipv4
```
The code was run again and there was no error
![Rerun](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/921e6043-5ef8-4724-84d3-d938527e4871)

 
