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

![Instance created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c33e4b72-de83-42f4-b77b-9263ae594f87)
![Security group](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2cf5d533-624c-4fc3-82f3-b43995b76cbf)

__2. Open up the Linux terminal to begin configuration.__
    - SSh into instance
    ```
    ssh -i "myinstance.pem" ec2-user@172-31-24-187
    ```
![Ec2 created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7ddb7593-0ba0-4dd0-a3fc-487c4a698721)

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
sudo gdisk /dev/nvme4n1
```
![partioning nvme4n1](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/6ba4026d-4b7d-44f9-8ecb-1698aa19c04b)
```
sudo gdisk /dev/nvme5n1
```
![partioning nvme5n1](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/70cef55c-be84-4d68-8534-d37c69d6958f)
```
sudo gdisk /dev/nvme6n1
```
![partioning nvme6n1](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/fe6a09f9-a4ac-4f4d-9a1f-ed70b0451b1d)

__5b. Use ```lsblk``` utility to view the newly configured partitions on each of the 3 disks__
```
lsblk
```
![Partitioned](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c8551fe7-e147-44b2-8f49-9dd8bddbb934)

__6. Install ```lvm``` package__
```
sudo yum install lvm2 -y
```
![sudo yum](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1543cf2e-a44a-46ea-8a73-4927a06db553)

__7. Use ```pvcreate``` utility to mark each of the 3 dicks as physical volumes (PVs) to be used by LVM. Verify that each of the volumes has been created successfully.__
```
sudo pvcreate /dev/nvme4n1p1 /dev/nvme5n1p1 /dev/nvme6n1p1

sudo pvs
```
![Physical volume created](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/19c5c67a-a915-4acb-805e-a358499ddc2d)

__8. Use ```vgcreate``` utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg. Verify that the VG has been created successfully__
```
sudo vgcreate webdata-vg /dev/nvme4n1p1 /dev/nvme5n1 /dev/nvme6n1

sudo vgs
```
![sudo vgs](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b61b4c41-2510-4d4a-a669-f74cd6e8a2e7)

__9. Use ```lvcreate``` utility to create 2 logical volumes, apps-lv (Use half of the PV size), and logs-lv (Use the remaining space of the PV size). Verify that the logical volumes have been created successfully.__
```
sudo lvcreate -n apps-lv -L 14G webdata-vg

sudo lvcreate -n logs-lv -L 14G webdata-vg

sudo lvs
```
![lvcreate](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/a0c58499-223d-4b77-b98a-8d33137e9583)

```apps-lv``` will be used to store data for the website while ```logs-lv``` will be used to store data for logs.

__10a. Verify the entire setup__
```
sudo vgdisplay -v   #view complete setup, VG, PV and LV
```
![vg display](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9b30eb08-f5be-4d37-8365-c9ae5e8805ab)
```
lsblk
```
![lsblk](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/b52727c2-331e-4538-98f4-86e09d46e304)

__10b. Use ```mkfs.ext4``` to format the logical volumes with ext4 filesystem__
```
sudo mkfs.ext4 /dev/webdata-vg/apps-lv

sudo mkfs.ext4 /dev/webdata-vg/logs-lv
```
![mfks](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/4024ed8c-6d4e-4691-ac86-68978bafc5a1)

__11. Create ```/var/www/html``` directory to store website files and ```/home/recovery/logs``` to store backup of log data__
```
sudo mkdir -p /var/www/html

sudo mkdir -p /home/recovery/logs
```
### Mount /var/www/html on apps-lv logical volume
```
sudo mount /dev/webdata-vg/apps-lv /var/www/html
```
![var html](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/6b4895f1-adec-495d-bff7-5163bdf3504c)

__12. Use ```rsync``` utility to backup all the files in the log directory ```/var/log``` into ```/home/recovery/logs``` (This is required before mounting the file system)__
```
sudo rsync -av /var/log /home/recovery/logs
```
![backup](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/5c1b07b8-e34a-46a6-8a1d-a468106fb6c1)

__13. Mount ```/var/log``` on ```logs-lv``` logical volume (All existing data on /var/log is deleted with this mounting process which was why the data was backed up)__
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
![Mount](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/0b81f603-af0e-4216-a09f-0d5460c6d47e)

__14. Restore log file back into ```/var/log``` directory__
```
sudo rsync -av /home/recovery/logs/log/ /var/log
```
![recover](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/0eaf0510-c4ae-4c72-81b6-31547f53291f)

__15. Update ```/etc/fstab``` file so that the mount configuration will persist after restart of the server__
```
sudo blkid  # To fetch the UUID

sudo vi /etc/fstab
```
```
df -h
```
![df h](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/628f1917-0d67-4cdf-a477-b17cf705011c)

## Step 2 - Prepare the Database Server
### Launch a second RedHat EC2 instance that will have a role - DB Server. Repeat the same steps as for the Web Server, but instead of ```apps-lv```, create ```dv-lv``` and mount it to ``` /db``` directory.

__1. Create 3 volumes in the same AZ as the DB Server ec2 each of 10GB and attache all 3 volumes one by one to the DB Server.__
![Db server](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/36c0baf3-9d09-4f76-aa60-c95a53e018df)
![Volume](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/12e54971-d778-4c1f-88cb-8b185c684bbc)

__2. Open up the Linux terminal to begin configuration.__
```
ssh -i "ec2pair.pem" ec2-user@3.208.15.164
```
![Db server connected](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/beafd616-5969-4a49-9e77-c7d12abf3e97)

__3. Use ```lsblk``` to inspect what block devices are attached to the server. Their name will likely be ```xvdf```, ```xvdg``` and ```xvdh```.__
```
lsblk
```
![xvdh](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/edb27327-299c-4c27-800c-4c403713dde5)

__4a. Use ```gdisk``` utility to create a single partition on each of the 3 disks.__
```
sudo gdisk /dev/xvdf
```
![Psrtitioning 1](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/698c4329-ff30-4754-b496-40dacdfb8825)
```
sudo gdisk /dev/xvdg
```
![Partitioning 2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/dca46dab-2223-4939-abf5-e30a7a7ec1f1)
```
sudo gdisk /dev/xvdh
```
![Partitioning 3](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/3a4779b1-8355-4f11-8f40-d37b4d3631f9)

__4b. Use ```lsblk``` utility to view the newly configured partitions on each of the 3 disks__
```
lsblk
```
![lsblk to check](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/4ff0a7cf-c799-4170-a5b6-83df0a0584b4)



