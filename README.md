# NSA-practical (these answers might have some errors)
## Configure and test an FTP Server


1. To install vsftpd, enter the command:

```
sudo yum install vsftpd
```

2. To launch the service and enable it at startup:
```
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```


Step 4: Create FTP User

Create a new FTP user with the following commands:
```
sudo useradd –m testuser
sudo passwd testuser
```
The system should ask you to create a password for the new testuser account. Create a sample
file in the new user’s home account:
```
sudo mkdir /home/testuser
```
Step 5: Configure Firewall to Allow FTP Traffic

Enter the following commands to open Ports 20 and 21 for FTP traffic:
```
sudo firewall-cmd --permanent --add-port=20/tcp
sudo firewall-cmd --permanent --add-port=21/tcp
sudo firewall-cmd --reload
```
Step 6: Connect to Ubuntu FTP Server

Connect to the FTP server with the following command:
```
ftp <your_centos_server_ip_or_hostname>
```


## Create a user and demonstrate Group administaation steps in CentOS.

### user creation
1. LOGIN as root in centos 7.
2. Navigate to home and create a user.
3. Provide a password for the given username.
4. To switch to that user enter the command
```
 su <username>.
```
### group management
1.Login as root and create a usergroup
```
sudo groupadd PRIME
```
2. Add the user in the created group.
```
sudo usermod –aG wheel <username>
```
3. To check  
```
sudo whoami
```

## Install, configure, and test the SAMBA server.

We need to create a directory for it to share:
```
mkdir /home/<username>/sambashare/
```

The configuration file for Samba is located at /etc/samba/smb.conf. To add the new directory as a share, we edit the file by running:
```
sudo nano /etc/samba/smb.conf
```
At the bottom of the file, add the following lines:
```
[sambashare]
 comment = Samba on Ubuntu
 path = /home/username/sambashare
 read only = no
 browsable = yes
```
Then press Ctrl-O to save and Ctrl-X to exit from the nano text editor. Now that we have our new share configured, save it and restart Samba for it to take effect:
```
sudo service smbd restart
sudo systemctl enable smb
```
Step 3: Update the firewall rules to allow Samba traffic:
```
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
```
Step 4: Setting up User Accounts and Connecting to Share

Since Samba doesn’t use the system account password, we need to set up a Samba password
for our user account:
```
sudo smbpasswd -a username
```
Step 5: Connecting to a Samba Share from Windows

Windows users also have an option to connect to the Samba share from both command line
and GUI. The steps below show how to access the share using the Windows File Explorer.

1. Open up File Explorer and in the left pane right-click on “This PC”.
2. Select “Choose a custom network location” and then click “Next”.
3. In “Internet or network address”, enter the address of the Samba share in the
following format \\samba_hostname_or_server_ip\sharename.


## Apache

Step 1: Installing Apache
```
sudo yum update
sudo yum install httpd
```
Step 2: Configuration of Firewall
```
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload
```
Step 3: Checking Web Server
Check with the systemd init system to make sure the service is running by typing:
```
sudo systemctl status httpd
```
Step 4: Starting Apache Service
If Apache is not already running, you can start it with:
```
sudo systemctl start httpd
```
Step 5: Enabling Apache to Start on Boot
```
sudo systemctl enable httpd
```
Step 6: Access the default Apache landing page
Access the default Apache landing page to confirm that the software is running properly through your IP address:


http://172.22.198.33/

## DNS 

Install BIND
```
sudo yum install bind bind-utils
```
Configure bind. Create or edit the BIND configuration file at /etc/named.conf.
Add following lines
```
zone "example.com" {
	type master;
	file "/var/named/example.com.zone";
};
```

4. Create the zone file specified (/var/named/example.com.zone)and configure it.
Modify /etc/named.conf.

 
Start and enable bind
```
systemctl start named
systemctl enable named
```
Add firewall ports
```
sudo firewall-cmd –permanent –add-services=dns
sudo firewall-cmd –reload
```
Test the DNS server
Open cmd on local machine and enter:

“ nslookup example.com”

## MySQL

Add the MySQL repository:
```
sudo yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```
Install MySQL server:
```
sudo yum install mysql-server
```
Step 2: Start and Enable MySQL Service
```
sudo systemctl start mysqld
sudo systemctl enable mysqld
```
Step 3: Secure MySQL Installation
Run the security script:
```
sudo mysql_secure_installation
```
Step 4: Get Temporary Root Password
Find the temporary root password:
```
sudo grep 'temporary password' /var/log/mysqld.log
```
Step 5: Log into MySQL
Log in with the temporary root password and change it:
```
mysql -u root -p
```
```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new_password';
```


Step 6: Create a New Database and User
Log in to MySQL:
```
mysql -u root -p
```
Create a new database:
```sql
CREATE DATABASE testdb;
```
Create a new user and grant privileges:
```sql
CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'user_password';
GRANT ALL PRIVILEGES ON testdb.* TO 'testuser'@'localhost';
FLUSH PRIVILEGES;
```
Replace user_password with a strong password.

Step 7: Test the MySQL Server
Log in as the new user:
```
mysql -u testuser -p
```
Select the database:
```sql
USE testdb;
Create a sample table and insert data:

CREATE TABLE sample_table (id INT AUTO_INCREMENT PRIMARY KEY, data VARCHAR(100));
INSERT INTO sample_table (data) VALUES ('Test Data');


SELECT * FROM sample_table;
```

## Proxy

Step 1: Install Squid
```
sudo yum install squid
```
Step 2: Start and Enable Squid Service
```
sudo systemctl start squid
sudo systemctl enable squid
```
Step 3: Configure Squid
Edit the Squid configuration file:
```
sudo nano /etc/squid/squid.conf
```
Look for the http_access directives and add the following line before the http_access deny all line:
```
http_access allow all
```
Step 4: Restart Squid Service
```
sudo systemctl restart squid
```
Step 5: Configure Firewall to Allow Squid Traffic
Open port 3128 for Squid:
```
sudo firewall-cmd --permanent --add-port=3128/tcp
sudo firewall-cmd --reload
```
Step 6: Test the Proxy Server
Configure your web browser to use the proxy server with the IP address of your CentOS server and port 3128.
Google Chrome:

- Open Chrome.
- Click on the three dots in the upper-right corner and select Settings.
- Scroll down and click on Advanced.
- Under System, click Open your computer's proxy settings.
- In the proxy settings window, go to Manual proxy setup.
- Turn on Use a proxy server and enter the IP address of your CentOS server and port 3128.
- Click Save.
- Then access any website and if the website work normally then the proxy server is working fine.

To check log
```
sudo tail -f /var/log/squid/access.log
```
