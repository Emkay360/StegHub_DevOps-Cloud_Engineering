## Side Study Documentation
### Network-attached storage (NAS)
Network-attached storage (NAS) is dedicated file storage that enables multiple users and heterogeneous client devices to retrieve data from a centralized disk capacity. Users on a local area network (LAN) access the shared storage via a standard Ethernet connection.

NAS devices typically don't have a keyboard or display and are configured and managed with a browser-based utility. Each NAS resides on the LAN as an independent network node, defined by its unique IP address.

![network_attached_storage](https://github.com/user-attachments/assets/05102e43-d311-4d5b-9c73-7d5180e2443d)

__NAS use cases and examples__

At home, people use a NAS system to store and serve multimedia files and to automate backups. Home users rely on network-attached storage to do the following:
- manage smart TV storage;
- manage security systems and security updates;
- manage consumer-based IoT components;
- create a media streaming service;
- manage torrent files;
- host a personal cloud server; and
- create, test, and develop a personal website.

 In the enterprise, NAS is used in the following ways:

- as a backup target, using a NAS array, for archiving and disaster recovery;
- for testing and developing web-based and server-side web applications;
- for hosting messaging applications;
- for hosting server-based, open-source applications, such as customer relationship management, human resource management, and enterprise resource planning applications; and
- for serving email, multimedia files, databases, and print jobs.

### Storage Area Network (SAN)

A __Storage Area Network (SAN)__ is a network of storage devices accessed by multiple servers or computers, providing a shared pool of storage space. Each computer on the network can access storage on the SAN as though they were local disks connected directly to the computer.

__SAN vs. NAS__

A SAN and network-attached storage (NAS) are two shared networked storage solutions. While a SAN is a local network composed of multiple devices, NAS is a single storage device that connects to a local area network (LAN). NAS is Ethernet-based, while SAN can use Ethernet and Fibre Channel. In addition, while SAN focuses on high performance and low latency, NAS focuses on ease of use, manageability, scalability, and lower total cost of ownership (TCO). Unlike SAN, NAS storage controllers partition the storage and then own the file system. Effectively this makes a NAS server look like a Windows or UNIX/Linux server to the server consuming the storage.




### Related Protocols
#### Network File System (NFS)
The Network File System (NFS) is a mechanism for storing files on a network. It is a distributed file system that allows users to access files and directories located on remote computers and treat those files and directories as if they were local.

For example, users can use operating system commands to create, remove, read, write, and set file attributes for remote files and directories.

#### Server Message Block (SMB)
The Server Message Block (SMB) protocol is a client-server communication protocol that is used for shared access to files, directories, printers, serial ports, and other resources on a network. It also provides an authenticated inter-process communication (IPC) mechanism.

#### File Transfer Protocol (FTP) and Secure FTP (sFTP)
FTP is a standard network protocol used to transfer files from one host to another over a TCP-based network. sFTP is a secure version of FTP that uses SSH to encrypt data transfers.

#### Internet Small Computer Systems Interface (iSCSI)
iSCSI is a transport layer protocol that works on top of the TCP/IP protocol, enabling the creation of storage area networks (SANs) by linking data storage facilities.

### Block-level Storage
Block-level storage refers to a type of storage typically used in SAN environments where data is stored in volumes, or blocks, and each block acts as an individual hard drive. This provides the flexibility to configure and manage storage as needed, allowing for high performance and low latency.

### AWS Services Examples
#### Block Storage
AWS provides block storage through its service called Amazon Elastic Block Store (EBS). EBS volumes act like raw, unformatted block devices and can be attached to EC2 instances, offering low-latency access to data.

#### Object Storage
Amazon Simple Storage Service (S3) is AWS's object storage service. It stores data as objects within resources called "buckets". S3 is designed for scalability, data availability, security, and performance.

#### Network File System
Amazon Elastic File System (EFS) is AWS's managed NFS service. It provides scalable file storage for use with AWS Cloud services and on-premises resources.

### Differences Between Block Storage, Object Storage, and Network File System
- __Block Storage:__

   - Uses raw storage volumes (blocks).
   - Ideal for applications requiring low latency and high performance.
   - Examples: Amazon EBS.

- __Object Storage:__

   - Stores data as objects in buckets.
   - Scalable and suitable for unstructured data.
   - Examples: Amazon S3.
    
- __Network File System:__

   - Manages shared file storage.
   - Suitable for workloads that require shared file access.
   - Examples: Amazon EFS.

## Conclusion
Understanding the differences between NAS, SAN, and related protocols, as well as the various types of storage provided by services like AWS, is crucial for designing and managing efficient and scalable storage solutions. Block-level storage offers flexibility and high performance, while object storage is ideal for scalability and managing large datasets. Network file systems provide shared file access, making them suitable for collaborative environments.
