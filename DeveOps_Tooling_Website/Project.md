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
-__Launch an EC2 Instance:__

- Create an EC2 instance with RHEL 8 Operating System to serve as the ```NFS server```.
- Create three 10GiB volumes in the same Availability Zone (AZ) as the ```NFS server```.
- Attach the volumes to the ```NFS server```

-__Open the Linux Terminal:__

-__SSH into the instance__
```
ssh -i ec2.pem ec2-user@3.237.94.123
```
![ssh key](https://github.com/user-attachments/assets/c5066d70-da0d-49fc-8332-63145ce63e68)

-__Create Partitions on Each Disk:__

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

-__Configure LVM on the Server:__

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
