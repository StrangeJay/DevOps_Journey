# DevOps_Journey

# **WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS** 
*LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)*


## **Step0:** Preparing Prerequisites

**Launching an EC2 instance**

- First you need to log into your AWS account, if you don't have one, you can sign up for a free tier account.
- Sign in to your AWS account and go to the search bar. Search for EC2 and click on it.
![Screenshot (167)](https://user-images.githubusercontent.com/105195327/205402536-d7ea64d6-4a20-4295-9739-7d31db590886.png)   
 
  
  
- It would lead you to the EC2 dashboard page, when there, click on launch instance. 
![Screenshot (168)](https://user-images.githubusercontent.com/105195327/205402731-aebac87d-5805-4bd7-8a0b-cc4dba3412b1.png)
 
  
  
- At the "Launch An Instance" page, set a name for your EC2 instance.
- Next, select an AMI for the instance.  
*An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance.*  
- You can choose from already existing AMI's or buy from the AWS market place.  
*For the purpose of this task, we will use the Ubuntu AMI.*   
![Screenshot (169)](https://user-images.githubusercontent.com/105195327/205409541-934e1ce4-0657-4c4a-9747-debf08a37bf7.png)
 
  
  
- Select an instance type.  
*We selected the t2.micro because it's covered by the AWS free tier plan, and we do not require much compute power for what we're about to do. 
You can choose your instance type depending on the computing power you need.*  
 ![Screenshot (170)](https://user-images.githubusercontent.com/105195327/205409743-50fa4165-030e-4546-8ab0-38e98d6ec9c7.png)  
  
   
   
- In the "Key Pair login" session, Create a key pair.  
*A key pair can be used to securely connect to your instance. If you already have an existing key pair, select it from the drop down menu.*  
![Screenshot (171)](https://user-images.githubusercontent.com/105195327/205410451-0c34846b-bb77-4f5e-a5fa-35fbf6643429.png)
 
  
  
- select the .pem private key format for open SSH, for linux users. And the .ppk for use with putty. I would be using putty to connect to my server.
- Click on create key pair, and the key would be created.
- Ensure to keep your private key safe, or else you won't be able to connect to your server again, once lost.

*** 

*Next is the Network settings.*  
- Leave the VPC and subnet on default setting, scroll down to the Firewall(Security Group) line.
- You could use an already existing Security Group(SG), or create a new one, depending on the needs of the server you'r creating.
![Screenshot (172)](https://user-images.githubusercontent.com/105195327/205431856-2cfcb7f2-faba-496b-9aff-b815352b5bf8.png)   
 
  
  
*For the purpose of this repo i'd be creating a new SG with inbound rule allowing SSH, HTTP and HTTPS access from anywhere...*  

![Screenshot (173)](https://user-images.githubusercontent.com/105195327/205432841-b25df0a3-323d-42ac-9378-14d0f478f980.png)
 
 
**Note:** Rules with source of 0.0.0.0/0 allow all IP addresses to access your instance. And if you take note of the  CIDR 
of our security group rules, they all have the 0.0.0.0/0 source, allowing access from all IP. 

- Click on the "Launch Instance" button, and your instance is created.
- Wait until it passes all checks, then your instance is ready for use.
- Select the instance and you  will see a dropdown menu with information, your public IP required to SSH is also included among the info.  

![Screenshot (175)](https://user-images.githubusercontent.com/105195327/205433645-512b90f8-544a-4f57-a751-ce98bea96865.png)

***
***

## **Step1**
### Installing Apache Server

*Using the public IP address from our previously created EC2 instance, we are going to use putty, to connect to the instance and install an Apache server on it.*
- Open the Putty application
- Input your public IP address in the Host Name(or IP name) box. 

![Screenshot (176)](https://user-images.githubusercontent.com/105195327/205437136-d9ac9551-cfa1-4101-b56a-c843f60259ce.png) 
 
  
  
- Click on "connection", change the section between keepalives to 30 

![Screenshot (177)](https://user-images.githubusercontent.com/105195327/205437239-18de4267-9202-4b73-96bd-a2a696cdec2d.png)  
 
  
 - Click the + sign beside the SSH button, a drop down menu would appear.
 - Click Auth, click the "browse" button below, beside "Private key file for authentication" 
![Screenshot (178)](https://user-images.githubusercontent.com/105195327/205437431-7121eb22-856f-462c-8424-a513654d7c66.png) 
 
  
 - Select your previously created .ppk file, and click on the open button below.  
   *Putty would show a dialog box asking for permission, select yes and you would be taking to a login page asking for authentication password.* 
 - Type "ubuntu", this is the password for the ubuntu AMI selected during the creation of the EC2 instance. 
 - You have been successfully connected to your EC2 instance.  
  
 ![Screenshot (179)](https://user-images.githubusercontent.com/105195327/205437638-c343f5a9-e756-441b-a980-87d35580e744.png)  
 
 
***

*Now we are going to install an Apache server on our EC2 instance.* 
We would be installing the Apache server using Ubuntu’s package manager ‘apt’. 

- Update a list of packages in package manager, using "Sudo apt update"
- Run apache2 package installation with "sudo apt install apache2" 
- When prompted, confirm installation by typing Y, and then ENTER

![Screenshot (180)](https://user-images.githubusercontent.com/105195327/205438244-a8b729a3-86c9-4043-a08e-c09c6d9b27f7.png)  
 
  
- After installation, to check if the Apache server is running as a service in our OS, we use the "sudo systemctl status apache2" 
*You should see a message similar to the one in the image below, telling you the Apache server is active and running.*  

![Screenshot (182)](https://user-images.githubusercontent.com/105195327/205438455-ef1b6c30-a182-4fb2-be04-1dbcacb4b3d8.png)  
 
- Use ctrl + c to exit the process
***

Now it is time to test how our Apache HTTP server can respond to requests from the Internet. 
- Open a web browser of your choice(i used chrome) and type in your EC2 public IP address, it should take you to the Apache server landing page. 

![Screenshot (183)](https://user-images.githubusercontent.com/105195327/205438778-c9d223b7-839a-4be6-a168-314c0d50b88e.png)
 
  
**Congratulations!!! Your Apache server is accessible from the internet.**   
***
*** 
  
## **Step2**
### Installing MYSQL

MySQL is a popular relational database management system used within PHP environments, and we shall be using it in our project. 
Once again, we would be using the ubuntu package manager 'apt' to make the installation. 

- Install mysql server using the command "sudo apt install mysql-server"
- When prompted, confirm installation by typing Y, and then ENTER.
- When the installation is finished, log in to the MySQL console by typing "sudo mysql", this would connect to the MySQL server as the root user

![Screenshot (185)](https://user-images.githubusercontent.com/105195327/205439563-4ef7f9ff-f366-4a12-88e9-0e9e639f3a79.png)  
 
  
  
It’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings 
and lock down access to your database system. 
Before running the script, you will set a password for the root user, using mysql_native_password as default authentication method. 
We’re defining this user’s password as **mysqltrial**  
- Exit the mysql shell with "exit" 

![Screenshot (187)](https://user-images.githubusercontent.com/105195327/205439954-3be7bb68-5bc7-4fb4-bf23-1b9d6b984135.png)  
 
  
  
- Start the interactive script by running: "sudo mysql_secure_installation"  
- When promptem for root password, input the previously created password, which in this case is **mysqltrial**
- You would receive a message asking you to validate password component  
  *You can choose to validate or not, dependong on how secure you want your password to be. 
  It checks the strength of a password, and allows users to set only passwords that are strong enough.  
  If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. 
  It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials. 
  
![Screenshot (188)](https://user-images.githubusercontent.com/105195327/205440336-c8e8b5e9-6b61-407c-8842-2c7ee29cbc24.png)  
 
  
*I am choosing to continue without enabling. I chose No, and continued the other processes.* 

- For the rest of the questions, press Y and hit the ENTER key at each prompt. 
*This will prompt you to change the root password, remove some anonymous users and the test database, 
disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.*  

![Screenshot (189)](https://user-images.githubusercontent.com/105195327/205440668-3739e71b-d925-4d83-a69b-90c7439d32be.png)  

 
- When you’re finished, test if you’re able to log in to the MySQL console by typing: "sudo mysql -p" 
![Screenshot (190)](https://user-images.githubusercontent.com/105195327/205440725-153c3d57-aa42-42c3-b2c3-04ae2b861ecd.png)  
 
 
*Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.* 
To exit the MySQL console, type:
mysql> exit

*** 
*** 

## **Step3**
### Installing PHP 

You have Apache installed to serve our content, and MySQL installed to store and manage your data. PHP is the component of your setup that will process code to display dynamic content to the end user. 
In addition to the php package, you’ll need php-mysql, *a PHP module that allows PHP to communicate with MySQL-based databases.* You’ll also need libapache2-mod-php to enable Apache to handle PHP files. *Core PHP packages will automatically be installed as dependencies.* 
To install these 3 packages at once, run: "sudo apt install php libapache2-mod-php php-mysql"

![Screenshot (191)](https://user-images.githubusercontent.com/105195327/205458912-d7e52c90-8cec-49df-b47e-0d0aa067c07f.png)  
 
  
 Once installation is finished, type "php -v" to confirm your php version. 
![Screenshot (193)](https://user-images.githubusercontent.com/105195327/205459017-75b8c788-b802-46d2-9aba-5be62e943f1a.png)  
 
  
  
At this point, your LAMP stack is completely installed and fully operational.
- [x] Linux (Ubuntu)
- [x] Apache HTTP Server
- [x] MySQL
- [x] PHP 

*** 
*** 

## **Step4**
### CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE 

In this project, you will set up a domain called **projectlamp**, but you can replace this with any domain of your choice.
*Apache on Ubuntu 20.04 has one server block enabled by default that is configured to serve documents from the /var/www/html directory.* 
We will leave this configuration as is and will add our own directory next to the default one. 

- Create the directory for projectlamp using ‘mkdir’ command as follows: "sudo mkdir /var/www/projectlamp" 
- Assign ownership of the directory with your current system user: "sudo chown -R $USER:$USER /var/www/projectlamp" 
- Create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. Here, we’ll be using nano. 
  sudo nano /etc/apache2/sites-available/projectlamp.conf

![Screenshot (194)](https://user-images.githubusercontent.com/105195327/205459726-29685e7f-9c77-4830-9c57-d84d26eff96e.png)  
 
  
This will create a new blank file. Paste in the following bare-bones configuration 
> <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

![Screenshot (195)](https://user-images.githubusercontent.com/105195327/205459887-93d353b8-1370-42d3-9288-2b0cc9204b69.png)  
 
  
 - To save the file, press ctrl + x, and yopu would be asked if you want to save, type **Y** for yes.  
 
 ![Screenshot (196)](https://user-images.githubusercontent.com/105195327/205460008-5310f425-84f1-4c0f-8afa-0e4a753d3014.png)  
  
   
 - Click enter at the name to write section to save as is. 
 
  ![Screenshot (197)](https://user-images.githubusercontent.com/105195327/205460028-a0292d77-e66f-49d9-9ebc-c894c63f6a6e.png)  
   
    
You can use the ls command to show the new file in the sites-available directory 
> sudo ls /etc/apache2/sites-available  
  
You will see something like this;

![Screenshot (198)](https://user-images.githubusercontent.com/105195327/205460189-31097b6c-afb4-47cb-9eff-7a1b45ef4e14.png)  
 
  
With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory. 
If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. *Adding the # character there will tell the program to skip processing the instructions on those lines.*
You can now use a2ensite command to enable the new virtual host:
> sudo a2ensite projectlamp  
 
You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website use a2dissite command, type:
> sudo a2dissite 000-default  
 
 
To make sure your configuration file doesn’t contain syntax errors, run:
> sudo apache2ctl configtest  
 

Finally, reload Apache so these changes take effect:
> sudo systemctl reload apache2  
 

![Screenshot (200)](https://user-images.githubusercontent.com/105195327/205460837-d44ee35a-a346-4cbe-b29c-17bfaaf21850.png)  
 
  
  
Your new website is now active, but the web root /var/www/projectlamp is still empty. 
*Create an index.html file in that location so that we can test that the virtual host works as expected:* 

sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
Now go to your browser and try to open your website URL using your public IP address. 
![Screenshot (201)](https://user-images.githubusercontent.com/105195327/205461002-a3cbea40-c293-40a0-853d-4fbd4c099aec.png)  
 
  
If you see the text from ‘echo’ command you wrote to index.html file, then it means your Apache virtual host is working as expected.
In the output you will see your server’s public hostname (DNS name) and public IP address. 
 
*You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.*  
 
  
*** 
*** 

## **Step5**
### ENABLE PHP ON THE WEBSITE  
 
 
With the default DirectoryIndex settings on Apache, a file named **index.html** will always take precedence over an **index.php file.** 
This is useful for setting up maintenance pages in PHP applications, by creating a temporary **index.html** file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:
> sudo nano /etc/apache2/mods-enabled/dir.conf  

> <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>  
 
![Screenshot (202)](https://user-images.githubusercontent.com/105195327/205461938-4b910bc1-94d6-42c5-b972-221d1424ad18.png)  


After saving and closing the file, you will need to reload Apache so the changes take effect:
> sudo systemctl reload apache2  


Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.
Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
*Create a new file named index.php inside your custom web root folder:*
> nano /var/www/projectlamp/index.php 


This will open a blank file. Add the following text, which is valid PHP code, inside the file:
 
![Screenshot (203)](https://user-images.githubusercontent.com/105195327/205462013-fe698daf-5b3f-42c6-a856-2cf87fa9712c.png)  
 
 

When you are finished, save and close the file, refresh the page and you will see a page similar to this:
  ![Screenshot (204)](https://user-images.githubusercontent.com/105195327/205462061-1e3b3ef4-2490-4bd9-8350-f6045513dd29.png)

 
*This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.* 
If you can see this page in your browser, then your PHP installation is working as expected.
After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment -and your Ubuntu server. You can use rm to do so:
> sudo rm /var/www/projectlamp/index.php



# THE END!!!
