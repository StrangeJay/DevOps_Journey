# DevOps_Journey

## **Step0:** Preparing Prerequisites

**Launching an EC2 server**

- First you need to log into your AWS account, if you don't have one, you can sign up for a free tier account. 
- Sign in to your AWS account and go to the search bar. Search for EC2 and click on it. 
![Screenshot (167)](https://user-images.githubusercontent.com/105195327/205402536-d7ea64d6-4a20-4295-9739-7d31db590886.png)


It would lead you to the EC2 dashboard page, when there, click on launch server. 
![Screenshot (168)](https://user-images.githubusercontent.com/105195327/205402731-aebac87d-5805-4bd7-8a0b-cc4dba3412b1.png)



- At the "Launch An Instance" page, set a name for your EC2 server.  
- Next, select an AMI for the server. 
An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance.
- You can choose from already existing AMI's or buy from the AWS market place. 
For the purpose of this task, we will use the Ubuntu AMI.   
![Screenshot (169)](https://user-images.githubusercontent.com/105195327/205409541-934e1ce4-0657-4c4a-9747-debf08a37bf7.png)



- Select an instance type. 
We selected the t2.micro because it's covered by the AWS free tier plan, and we do not require much compute power for what we're about to do. 
You can choose your instance type depending on the computing power you need.
 ![Screenshot (170)](https://user-images.githubusercontent.com/105195327/205409743-50fa4165-030e-4546-8ab0-38e98d6ec9c7.png)



- In the "Key Pair login" session, Create a key pair. 
A key pair can be used to securely connect to your instance. If you already have an existing key pair, select it from the drop down menu.
![Screenshot (171)](https://user-images.githubusercontent.com/105195327/205410451-0c34846b-bb77-4f5e-a5fa-35fbf6643429.png)



- select the .pem private key format for open SSH, for linux users. And the .ppk for use with putty. I would be using putty to connect to my server.
- Click on create key pair, and the key would be created. 
- Ensure to keep your private key safe, or else you won't be able to connect to your server again, once lost.

*** 

Next is the Network settings. 
- Leave the VPC and subnet on default setting, scroll down to the Firewall(Security Group) line. 
- You could use an already existing Security Group(SG), or create a new one, depending on the needs of the server you'r creating.  
![Screenshot (172)](https://user-images.githubusercontent.com/105195327/205431856-2cfcb7f2-faba-496b-9aff-b815352b5bf8.png)


For the purpose of this repo i'd be creating a new SG with inbound rule allowing SSH, HTTP and HTTPS access from anywhere...

![Screenshot (173)](https://user-images.githubusercontent.com/105195327/205432841-b25df0a3-323d-42ac-9378-14d0f478f980.png)

**Note:** Rules with source of 0.0.0.0/0 allow all IP addresses to access your instance. And if you take note of the  CIDR 
of our security group rules, they all have the 0.0.0.0/0 source, allowing access from all IP.

- Click on the "Launch Instance" button, and your instance is created. 
- Wait until it passes all checks, then your instance is ready for use. 
- Select the instance and you  will see a dropdown menu with information, your public IP required to SSH is also included among the info. 

![Screenshot (175)](https://user-images.githubusercontent.com/105195327/205433645-512b90f8-544a-4f57-a751-ce98bea96865.png)

***
***


