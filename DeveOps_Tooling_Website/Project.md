## DevOps Tooling Website Solution Project
### Overview

![3 tier application](https://github.com/user-attachments/assets/b28f34fa-b50a-4b0d-8d34-78ac316ee7e2)

As a DevOps team member, you will implement a tooling website solution that provides easy access to DevOps tools within the corporate infrastructure. This project involves setting up an infrastructure consisting of:

1. Infrastructure: AWS
2. Web Server: Red Hat Enterprise Linux 8
3. Database Server: Ubuntu 24.04 + MySQL
4. Storage Server: Red Hat Enterprise Linux 8 + NFS
5. Programming Language: PHP
6. Code Repository: GitHub

## Step 1: Prepare NFS Server
- __Launch an EC2 Instance:__

- Create an EC2 instance with RHEL 8 Operating System to serve as the ```NFS server```.
- Create three 10GiB volumes in the same Availability Zone (AZ) as the ```NFS server```.
- Attach the volumes to the ```NFS server```

- __Open the Linux Terminal:__

- __SSH into the instance__
```
ssh -i ec2.pem ec2-user@3.237.94.123
```
![ssh key](https://github.com/user-attachments/assets/c5066d70-da0d-49fc-8332-63145ce63e68)

- __Create Partitions on Each Disk:__

Use ```gdisk``` to create a single partition on each of the three disks:
```
sudo gdisk /dev/nvme1n1
sudo gdisk /dev/nvme2n1
sudo gdisk /dev/nvme3n1
```
Type n to create a new partition.
click enter till you reach the Hex code or GUID and type 8e00
Click on Enter to accept default settings till it prompts for a command again.
Type w to write.
Then y to overwrite the partition.
Repeat the same process for other disks

![partitioned disks](https://github.com/user-attachments/assets/a21b7215-9b28-415f-90b6-56bb7c596e82)

- __Configure LVM on the Server:__

- First, we install the lvm2 service.
```
sudo yum install lvm2 -y
sudo lvmdiskscan
```
- Create physical volumes, a volume group, and logical volumes:
```
sudo pvcreate /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
sudo lvcreate -n opt-lv -L 10G webdata-vg
sudo lvcreate -n apps-lv -L 10G webdata-vg
sudo lvcreate -n logs-lv -L 8G webdata-vg
```
![pvc](https://github.com/user-attachments/assets/f7bb5d29-5b31-45d1-bc2e-edca671868e9)
![L volumes](https://github.com/user-attachments/assets/75955d95-653f-4bc5-8745-4f348412a426)

- Format the logical volumes with ```xfs```:
```
sudo mkfs.xfs /dev/webdata-vg/opt-lv
sudo mkfs.xfs /dev/webdata-vg/apps-lv
sudo mkfs.xfs /dev/webdata-vg/logs-lv
```
![format volumes](https://github.com/user-attachments/assets/1f5882cb-d3e1-40ad-9d7c-f088f5dfe851)

- Create mount points and mount the logical volumes:
```
sudo mkdir -p /mnt/apps /mnt/logs /mnt/opt
sudo mount /dev/webdata-vg/apps-lv /mnt/apps
sudo mount /dev/webdata-vg/logs-lv /mnt/logs
sudo mount /dev/webdata-vg/opt-lv /mnt/opt
```
![mnt](https://github.com/user-attachments/assets/ea419f32-7a97-418a-befc-9f0d66909ce2)

- __Install and Configure NFS Server:__
```
sudo yum update -y
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![nfs started](https://github.com/user-attachments/assets/16bb1784-2a8e-4917-b95a-1cd08045c771)

- Set permissions for the mount points:
```
sudo chown -R nobody: /mnt/apps /mnt/logs /mnt/opt
sudo chmod -R 777 /mnt/apps /mnt/logs /mnt/opt
sudo systemctl restart nfs-server.service
```
![image](https://github.com/user-attachments/assets/a7b0af34-2f3a-4ef2-93f7-097f4255bbf5)

- Configure NFS exports:
```
/mnt/apps 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.0.0/20(rw,sync,no_all_squash,no_root_squash)
```
Save and apply the changes:
```
sudo exportfs -arv
```
![arv export](https://github.com/user-attachments/assets/178c1db0-0855-4300-983a-fb6ddd293841)

- __Open NFS Ports:__

- Check NFS ports and open them in the security group:
```
rpcinfo -p | grep nfs
```
![Open port](https://github.com/user-attachments/assets/81e7741c-e397-4059-ab16-1295633ed150)

- Open ports TCP 111, UDP 111, and UDP 2049 in the security group.

![sec group](https://github.com/user-attachments/assets/234caaed-a1bb-4332-8841-2720d8a31470)

## Step 2: Configure the Database Server
- __SSH into the instance__
```
ssh -i ec2pair.pem ubuntu@54.152.130.212
```
- __Install MySQL Server on Ubuntu 24.04:__

- Update and install MySQL server:
```
sudo apt update && sudo apt upgrade -y
sudo apt install mysql-server -y
```
- __Create a Database and User:__

- Log in to MySQL and create a database and user:
```
sudo mysql
```
```
CREATE DATABASE tooling;
CREATE USER 'webaccess'@'172.31.0.0/20' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON tooling.* TO 'webaccess'@'172.31.0.0/20';
FLUSH PRIVILEGES;
EXIT;
```
![mysql serv](https://github.com/user-attachments/assets/1c1ca5d0-c6aa-4c48-83d9-86bf5fadf5af)

- Edit the MySQL configuration file to bind it to all IP addresses, (0.0.0.0) Open the MySQL configuration file, which is located at ```/etc/mysql/mysql.conf.d/mysqld.cnf```
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```
- Start and enable MySQL service:
```
sudo systemctl restart mysql
sudo systemctl enable mysql
sudo systemctl status mysql
```
![restart mysql](https://github.com/user-attachments/assets/b0bea22a-9cf0-4948-acc8-503c1fa462ba)

## Step 3: Prepare the Web Servers
- __SSH into the instance__
```
ssh -i keypair.pem ec2-user@100.24.116.231
```
- __Configure NFS Client on All Web Servers:__

- Launch (3) new EC2 instances with RHEL 8 OS.
- Install NFS client:
```
sudo yum update -y
sudo yum install nfs-utils nfs4-acl-tools -y
```
- Mount NFS Web shares ```/var/www```:
```
sudo mkdir -p /var/www
sudo mount -t nfs -o rw,nosuid 172.31.11.64:/mnt/apps /var/www
```  
- Add the following lines to /etc/fstab to ensure mounts persist after reboot:
```
sudo vi /etc/fstab
```
Add:
```
172.31.11.64:/mnt/apps /var/www nfs defaults 0 0
```
![mntg](https://github.com/user-attachments/assets/59eec301-e7e4-4699-8a02-786dc3b5ad3d)

- __Install Apache and PHP:__

- Install Remi's repository, Apache, and PHP:
```
sudo yum install httpd -y
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm -y 
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm -y 
sudo dnf module reset php -y
sudo dnf module enable php:remi-7.4 -y
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd -y
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo setsebool -P httpd_execmem 1
```
- Start and enable Apache:
```
sudo systemctl start httpd
sudo systemctl enable httpd
```
- Mount NFS Logs shares ```/var/log/httpd```:
```
sudo mkdir -p /var/log/httpd
sudo mount -t nfs -o rw,nosuid 172.31.11.64:/mnt/logs /var/log/httpd
```
- Add the following lines to ```/etc/fstab``` to ensure mounts persist after reboot:
```
sudo vi /etc/fstab
```
Add:
```
172.31.11.64:/mnt/logs /var/log/httpd nfs defaults 0 0
```
- __Deploy Tooling Application:__

 - Fork the tooling source code from Darey.io GitHub account to your GitHub account.

![Fork](https://github.com/user-attachments/assets/cf64b074-8a2a-44d8-bdb3-de43fc48c0f2)

 - Then clone the source code repository to the Web server.
```
sudo yum install git -y
git clone https://github.com/alagbaski/tooling.git
```
 - Deploy the code to the web server and ensure the repository's ```html``` folder is deployed to ```/var/www/html```.
```
cd tooling/html
sudo mv * /var/www/html/
```
 - Verify that Apache files and directories are available on the web server in ```/var/www``` and on the NFS server ```/mnt/apps```.
 - If you see the same files, it means NFS is mounted correctly. Create a new file ```touch test.txt``` from one server and check if it is accessible from other web servers.

- __Configure Database Connection for the Website:__

  - Update the website's configuration to connect to the database (in the ```/var/www/html/functions.php``` file).
 
