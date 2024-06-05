## Web Solution With WordPress: Implementing a Basic Web Solution Using WordPress on Red Hat Linux
### Overview
This project involves setting up storage infrastructure on two Linux servers and implementing a basic WordPress web solution. It consists of two main parts:

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

__2. Open up the Linux terminal to begin configuration.__
    - SSh into instance
    ```
    ssh -i "emkay.pem" ubuntu@ec2-54-158-25-148.compute-1.amazonaws.com
    ```
![ssh client](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/bb44bd12-d782-4b5a-9251-2f78826cc568)

__3. Use ```lsblk``` to inspect what block devices are attached to the server. All devices in Linux reside in /dev/ directory. Inspect with ```ls /dev/``` and ensure all 3 newly created devices are there. Their name will likely be ```xvdf```, ```xvdg``` and ```xvdh```.__
```
lsblk
```
![showing volumes](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/92f93291-466e-4c68-b14e-6064d111cda6)

__4. Use ```df -h``` to see all mounts and free space on the server.__
```
df -h
```
![available disk](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/f489a137-7fd5-4c58-a586-14495ad93186)

__5a. Use ```gdisk``` utility to create a single partition on each of the 3 disks.__
```
sudo gdisk /dev/xvdf
```
![Sudo disk](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3559b156-ebb7-452c-ad67-eb2d4bc07437)
```
sudo gdisk /dev/xvdg
```
![gdisk for g](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/32837244-e4ae-48e9-a29c-946b758ac154)
```
sudo gdisk /dev/xvde
```
![gdisk for e](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/4cbf7a59-2dbd-4b44-b90c-c595cb49e038)

__5b. Use ```lsblk``` utility to view the newly configured partitions on each of the 3 disks__
```
lsblk
```
![partitioning](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/351f1ed6-100f-4da1-9065-a30b786a132b)
