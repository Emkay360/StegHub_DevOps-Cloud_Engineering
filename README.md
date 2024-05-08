# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS 103
## Introduction:
LAMP is an open-sourced web development used by developers. It consists of 4 components: Linux (Operating system), Apache (web server software that delivers content through the internet), MySQL (An open source relational database for storing applications), PHP (Sometimes Perl or Python, a programming language that helps Apache in creating dynamic web pages). 
# Step 0: Prerequisites
**1.** EC2 instance of t2.micro type and Ubuntu 24.04 LTS (HVM) was launched in us-east-1 region using the AWS console  

![Screenshot (138)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/549148bd-6768-4e22-b09c-75e5108770d8)
![Screenshot (139)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7805438c-c68e-40a5-ad7d-0bcb283669e3)  

**2.** Created a new SSH key pair to access the instance on port 22

**3.** The security group was configured using the following inbound rules:

- Allow traffic on port 80 (HTTP) with source from anywhere on the internet.
- Allow traffic on port 443 (HTTPS) with source from anywhere on the internet.
- Allow traffic on port 22 (SSH) with source from any IP address. This is opened by default
  
![Screenshot (140)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2ec68129-cda8-4f29-a419-fc75b5c6059e)

**4.** For the network configuration, the default VPS subnet was used.

![Screenshot (141)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/2bf58110-92d1-4e02-98f1-1283f9adeaad)

**5.** The private ssh key downloaded was used to connect to the instance.

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

**2.** **Enable and verify Apache2**
If it is green, then it is running well
```
sudo systemctl enable
sudo systemctl status apache2
```
![apache2](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/8efaf5ae-e567-4463-84b9-0b29e8fa0e36)

**3.** **Checking if the server is running and can be accessed locally through the Ubuntu Shell**
```
curl http://localhost:80
```
![Local host](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1ed7352d-4489-45f3-80a0-16845c5703b0)

**4.** **Testing how our Apache server can respond from the internet**
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

## Step 2: Installing MySQL
**1.** **Installed MySQL database**
MySQL server was installed successfully
```
sudo apt install mysql-server
```
![Installed MySQL server](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/837381f0-1725-4cb4-82d7-d5de74287020)
When prompted, the installation was confirmed by y

**2.** **MySQL was verified**
```
sudo systemctl status Mysql
```
![Screenshot (163)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/e71a330b-1664-492f-b5bb-5cd5ea14795e)

**3.** **Logged into MySQL**
```
sudo mysql
```
This is connected to the MySQL administrative database user **root**, which is inferred by the use of sudo when running this command.

**4.** **Set password using mysql_native_password**
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Admin123$';
```
![MySQL password added](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/49ea35bd-2d41-4125-b11b-b235456fc6d0)
The user password was defined by 'Emkay360' and exit MySQL

**5.** **Running interactive script**
```
sudo mysql_secure_installation
```
![running interactive script](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9719e86e-5ca3-41f5-8480-8d7574523e14)
Regardless of whether you choose to set up the **validate password plugin* the server will next ask you to select and confirm a password for the MySQL root user.

**6.** **After you select a new password, you log in to the MySQL console**
```
sudo mysql -p
```
![logged into mysql with new password](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/9dcdb507-fa97-4951-9ceb-3fc6aef68963)
Logged in to the console once again which required a password to connect as the root user

## Step 3: Installing PHP
**1.** **Installed PHP**
PHP Module allows PHP to communicate with MySQL-based databases 
The following was installed together with PHP module.

- PHP package
- php-mysql, a module that allows php to communicate with mysql-based databases.
- libapache2-mod, a package that allows Apache to handle PHP files.
  ```
  sudo apt install php libapache2-mod-php php-mysql
  ```
  ![Installing php](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7ef871d8-5b86-4547-9eb8-971f1b055e72)

  Confirm PHP version
  ```
  php -v
  ```
  ![php -v](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/c729233d-b5d4-4a30-aca5-993cfa63ed1b)

  ## Step 4: Creating a Virtual Host for the website using Apache
  **1.** **Apache on Ubuntu 20.04 has a default server block enabled and configured to serve /var/www/html. Adding a directory next to the default one**
  Creating a directory using **'mkdir* command.
  ```
  sudo mkdir /var/www/projectlamp
  ```
  **Assigning ownership to the directory with $USER environment variable which references the current system user.**
  ```
  sudo chown -R $USER:$USER /var/www/projectlamp
  ```
  ![adding our own directory](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7f3630a3-537e-4851-880b-44e7445974b7)
**2.** **Create and open a new configuration file in Apache** **sites-available*
```
sudo vim /etc/apache2/sites-available/projectlamp.conf
```
Pasting bare-bones configuration below:
```
<VirtualHost *:80>
  ServerName projectlamp
  ServerAlias www.projectlamp
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/projectlamp
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
![Boner](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/85c768dd-8e8c-41eb-b409-d9bb3352f5d9)
**3.** **Show the new file in the site-available directory**
```
sudo ls /etc/apache2/sites-available
```
![Conf](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/25e5962e-736b-4139-a08f-b0d67832edb8)

**4.** **Enable the virtual host**
```
sudo a2ensite projectlamp
```
![enable host](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7ca9a43a-ef38-4397-b98d-5fbe0e14d331)

**5.** **Disabling Apache default website**
If the default website is not disabled, the Apache default configuration would overwrite the virtual host.
```
sudo a2dissite 000-default
```
![dissite](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/7346db43-9ebd-4b8b-8c08-3cea3af15a94)
**6.** **Ensure the configuration doesn't contain a syntax error, run:**
```
sudo apache2ctl configtest
```
![syntax ok](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/671ce32a-f8ce-4ffc-a4c5-db97fbad15c4)
**7.** **Reload Apache**
```
sudo systemctl reload apache2
```
**8.** **The new website is now active, but the web root /var/www/projectlamp is still empty**
Create an index.html in the location so that a test can be carried out on the virtual to ensure it's working
```
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
```
![Hello lamp](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/1a45d466-2242-4c35-be2c-f2d69c477c8c)
**9.** **Tried opening the public address on the web browser**
```
HTTP://http://3.93.185.64:80
```
![Screenshot (167)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/738f16cc-17a5-44aa-a695-b8aad307ce8f)

**10.** **Accessing the website through public DNS**
```
http://<public-DNS-name>:80
```
![Screenshot (168)](https://github.com/Emkay360/StegHub_DevOps-Cloud_Engineering/assets/56301419/91dce020-a7e7-4a64-83b9-535e5ef52720)

