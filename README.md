# DevOps_Journey

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
 
  
 


