---
title: Full Installation Documentation
description: Guide on Full Vanguard Financials Installation
published: true
date: 2024-04-22T12:22:30.174Z
tags: installation, vfin, documentation, guide, vanguard, financials
editor: markdown
dateCreated: 2024-04-22T12:22:26.692Z
---

# Header



Phoenix Deployment (v2.0)

Table of Contents
1.	Prerequisites
1.	Windows Features Requirements
2.	Redis Server
3.	Erlang
4.	RabbitMQ
5.	Systems Frameworks Requirements
6.	Host bundle Installation
2.	Systems Installation
1.	WCF
1.	Publish WCF
2.	Host on IIS
3.	Test WCF
2.	Identity Server
1.	Publish Identity Server
2.	Deploy on IIS
3.	Test Identity Server
3.	Worker Service
1.	Publish Worker Service
2.	Run as a Windows Service
4.	Web API
1.	Publish Web API
2.	Host on IIS
3.	Test Web API
4.	Debug Web API
5.	React Web App
1.	Publish React Web App
2.	Deploy on IIS
3.	Debug API
3.	Run Web Application
4.	Systems updates
5.	Automation
6.	References
7.	Index
1.	Glossary

Prerequisites

Before deploying the system, it's crucial to ensure that the required prerequisites are met. Prerequisites are essential components, services, or configurations that lay the foundation for the successful deployment and operation of the system.
They encompass various aspects, such as software dependencies, server configurations, and external services. Properly meeting these prerequisites ensures that the system can seamlessly integrate with its dependencies. This reduces the likelihood of deployment failures/errors and/or runtime issues and longer diagnostics on any subsystems.
 
Turn on Windows Features
In this section, there’s an outline of specific Windows features that are required for the system. Ensuring the presence and correct configuration of these features is essential for a successful deployment on a Windows environment.
 
Windows PC version

In the windows search tab on the task bar, search for 
“Turn windows Features On or Off”
Click Open to open the Windows Features window as shown below. 
Proceed to turn on all features for: 
•	.NET framework. 
•	Internet Information Services & Internet Information Services Hostable Web Core. 
•	Microsoft Message Queue (MSMQ) Server













Windows Server
In a windows Server environment
On the Windows Search tab, type ‘Server Manager’ and click on the settings Server manager that appears as shown below.


Once the server manager has launched, on the top right, look for manage, then ‘Add roles and features’ as shown in the image below
This should open a wizard for adding Roles and Features. 
In the wizard, proceed through the various stages
a.	‘Before you begin’ – this lets you verify that server requirements like admin account setups and networks have been made correctly etc. 
b.	‘Installation Type’ – Choose ‘Role -based or feature-based installation’
c.	‘Server Selection’ – Leave it as default option, - select a server from the server pool
d.	Server Roles – skip this 
e.	Server Features – Here, select the features as indicated in section (1.1.1) above i.e. 
	.NET framework. 
	Internet Information Services & Internet Information Services Hostable Web Core. 
	Microsoft Message Queue (MSMQ) Server
See image below: 

Redis Server Installation and configuration
This can be done both on a Linux server or in a windows environment. 

Windows environment
Download a Redis windows installer from trusted site. Run the installer and install it with default configurations. 
Download Link: https://github.com/tporadowski/redis/releases 


Once done, a Redis service will be created. Redis configurations can be made in the redis.windows-service.conf file located at the root installation folder that Redis is installed.
Some of the configurations done are; 
	Set binding for different IP/hostnames
	TCP port
	Redis Password
	Protected mode etc.

Linux Environment
< Disclaimer> Documentation on this will be updated in due time 

Erlang/OTP Installation Guide
Installing the Erlang/OTP has a relatively straight forward approach. Head over to the Erlang downloads page using the link provided: https://www.erlang.org/downloads
Head over to click on the windows installer as shown below;


This should prompt a download via the default browser downloader or a download manager if there is one installed in your PC. 
Check the images below for reference. 





(No.1)


(No. 2)




Proceed to run the installer. The default configurations are ideal for any setup not unless you want to change the installation path. 


You can alter to make sure it includes the Microsoft DLL files, 
NB:// Make sure you are aware of the changes you make for easier debug and configuration management.

For the ideal setup proceed to have the Erlang/OTP installed with the default settings/ pre-configured choices and click finish when prompted.


RabbitMQ Installation Guideline
Rabbitmq is an open-source message broker that facilitates communication between various components of a software application. It uses a publish-subscribe pattern enabling different sections of the application to have asynchronous and decoupled communication from each other.




Installation of Rabbitmq has prerequisites; 

Once the Erlang installation is complete, proceed to https://www.rabbitmq.com/download.html to download the Rabbitmq installation file
In the download page check under ‘Downloading and Installing RabbitMQ >> Open Source RabbitMQ Server’  under windows, choose either of the file types indicated.

 
Once the download is complete, run it as an administrator. NB//: Running as administrator assists avoid error while starting the management service.

 
Proceed and do the installation without changing the default settings: //You can change the default installation path according to preference.

After the installation, the RabbitMQ service starts. 
To verify that service starts and get the Navigate to the installation folder to the ‘sbin’ folder  as shown:  
C:\Program Files\RabbitMQ Server\rabbitmq_server-3.12.4\sbin and open in terminal. 
Run the command 
‘rabbitmq-plugins enable rabbitmq_management’ as shown below

 
Next head on to Windows Services and restart the RabbitMQ Service. Once restarted, proceed to your browser and visit the default RabbitMQ management url ‘http://localhost:15672’

 

In some Scenarios, some machines resolve the management UI screen as shown above after restarting the service.  Other Machines may fail to resolve this instantly and thus editing the hosts file with rabbitmq nameserver solves this.
Navigate to Location (C:\Windows\System32\drivers\etc\hosts) and add
127.0.0.1 rabbitmq to the list of network configs in the file.
This should help resolve the issue. Retry the rabbitmq default url to access the management UI page.

The default login credentials for this management portal are;
Username: guest
Password: guest

This should open the default management page as shown below.
 




Navigate to the Admin tab.
In the extreme right look for Virtual hosts. You need to create Vfin_WebApp’ Virtual host.

 

The resulting page after clicking Virtual hosts should be as shown below;

 



Proceed to click on the ‘Add a new virtual host’ and then fill in the ‘Name’ and ‘Description’ then click ‘Add Virtual Host’ as shown below;

 




Once you have created a virtual host, the result should be as shown below;

 

Systems Frameworks and Hosting Bundle
These tools and components are essential for developing, deploying, and running .NET Core and ASP.NET Core applications. In these sections their use is purposely for running the system in deployment mode.

1.	NET Core SDK:
•	The .NET Core SDK (Software Development Kit) is a set of tools that enables developers to build .NET Core applications. It includes the .NET Core runtime, libraries, command-line tools, and other resources necessary for developing and compiling .NET Core applications. The SDK is required for developing new applications and maintaining existing ones.

2.	.NET Core Runtime:
•	The .NET Core Runtime is the environment required to run .NET Core applications. It includes the .NET Core libraries and runtime components necessary for executing .NET Core code. The runtime is necessary for deploying and running .NET Core applications on a server or other computing environment.

3.	ASP.NET Core Runtime:
•	The ASP.NET Core Runtime is a subset of the .NET Core Runtime specifically tailored for running ASP.NET Core web applications. It includes additional components and libraries necessary for hosting and serving ASP.NET Core web applications, such as the Kestrel web server.

4.	Hosting Bundle:
•	The Hosting Bundle is a package that includes the .NET Core Runtime, ASP.NET Core Runtime, and additional dependencies required for hosting and running .NET Core or ASP.NET Core applications on a server or other production environment. It simplifies the deployment process by bundling all necessary components into a single package.

In the deployment environment, ensure you install the following SDKs, .NET Core frameworks, and the hosting bundle as listed below.
•	Microsoft .NET SDK 8.0 
•	Microsoft .NET SDK 6.0
•	Microsoft ASP.NET Core 6.0
•	Microsoft ASP.NET Core 8.0
•	Microsoft .NET Runtime – 6.0
•	Microsoft .NET Runtime – 8.0
•	Microsoft .NET 8.0.1 Windows Server Hosting




Systems Installation
For a new installation, prepare the Vanguard Financials appropriate directories. These determine where different services and Vanguard Financials resources can be located by sub-systems and users for normal operations and diagnostics.

Running Vanguard Financials recommends splitting the main server / Widows PC disk into more than one partitions. 
This enhances: 
	Disk optimization to improve efficiency
	Improved Performance
	Reduce risks of data loss when 1 partition fails
	Selective and Quicker backups and restores 
	Granular Recovery options 




Automated File Creation
Use the automating tool to create the necessary folders.
Proceed and run vfin_DirSort script to generate all the folders and sub-folders accordingly.
 
Incase the tool is under maintenance or does not work, go ahead and create ‘VanguardFinancials’ folder in your Secondary drive e.g. 
D:\VanguardFinancials or E:\VanguardFinancials.
Then proceed and create the various subfolders as shown in the image below;







Windows Server
In a windows Server environment
On the Windows Search tab, type ‘Server Manager’ and click on the settings Server manager that appears as shown below.


Once the server manager has launched, on the top right, look for manage, then ‘Add roles and features’ as shown in the image below
This should open a wizard for adding Roles and Features. 
In the wizard, proceed through the various stages
a.	‘Before you begin’ – this lets you verify that server requirements like admin account setups and networks have been made correctly etc. 
b.	‘Installation Type’ – Choose ‘Role -based or feature-based installation’
c.	‘Server Selection’ – Leave it as default option, - select a server from the server pool
d.	Server Roles – skip this 
e.	Server Features – Here, select the features as indicated in section (1.1.1) above i.e. 
	.NET framework. 
	Internet Information Services & Internet Information Services Hostable Web Core. 
	Microsoft Message Queue (MSMQ) Server
See image below: 

Redis Server Installation and configuration
This can be done both on a Linux server or in a windows environment. 

Windows environment
Download a Redis windows installer from trusted site. Run the installer and install it with default configurations. 
Download Link: https://github.com/tporadowski/redis/releases 


Once done, a Redis service will be created. Redis configurations can be made in the redis.windows-service.conf file located at the root installation folder that Redis is installed.
Some of the configurations done are; 
	Set binding for different IP/hostnames
	TCP port
	Redis Password
	Protected mode etc.

Linux Environment
< Disclaimer> Documentation on this will be updated in due time 

Erlang/OTP Installation Guide
Installing the Erlang/OTP has a relatively straight forward approach. Head over to the Erlang downloads page using the link provided: https://www.erlang.org/downloads
Head over to click on the windows installer as shown below;


This should prompt a download via the default browser downloader or a download manager if there is one installed in your PC. 
Check the images below for reference. 





(No.1)


(No. 2)




Proceed to run the installer. The default configurations are ideal for any setup not unless you want to change the installation path. 


You can alter to make sure it includes the Microsoft DLL files, 
NB:// Make sure you are aware of the changes you make for easier debug and configuration management.

For the ideal setup proceed to have the Erlang/OTP installed with the default settings/ pre-configured choices and click finish when prompted.


RabbitMQ Installation Guideline
Rabbitmq is an open-source message broker that facilitates communication between various components of a software application. It uses a publish-subscribe pattern enabling different sections of the application to have asynchronous and decoupled communication from each other.




Installation of Rabbitmq has prerequisites; 

Once the Erlang installation is complete, proceed to https://www.rabbitmq.com/download.html to download the Rabbitmq installation file
In the download page check under ‘Downloading and Installing RabbitMQ >> Open Source RabbitMQ Server’  under windows, choose either of the file types indicated.

 
Once the download is complete, run it as an administrator. NB//: Running as administrator assists avoid error while starting the management service.

 
Proceed and do the installation without changing the default settings: //You can change the default installation path according to preference.

After the installation, the RabbitMQ service starts. 
To verify that service starts and get the Navigate to the installation folder to the ‘sbin’ folder  as shown:  
C:\Program Files\RabbitMQ Server\rabbitmq_server-3.12.4\sbin and open in terminal. 
Run the command 
‘rabbitmq-plugins enable rabbitmq_management’ as shown below

 
Next head on to Windows Services and restart the RabbitMQ Service. Once restarted, proceed to your browser and visit the default RabbitMQ management url ‘http://localhost:15672’

 

In some Scenarios, some machines resolve the management UI screen as shown above after restarting the service.  Other Machines may fail to resolve this instantly and thus editing the hosts file with rabbitmq nameserver solves this.
Navigate to Location (C:\Windows\System32\drivers\etc\hosts) and add
127.0.0.1 rabbitmq to the list of network configs in the file.
This should help resolve the issue. Retry the rabbitmq default url to access the management UI page.

The default login credentials for this management portal are;
Username: guest
Password: guest

This should open the default management page as shown below.
 




Navigate to the Admin tab.
In the extreme right look for Virtual hosts. You need to create Vfin_WebApp’ Virtual host.

 

The resulting page after clicking Virtual hosts should be as shown below;

 



Proceed to click on the ‘Add a new virtual host’ and then fill in the ‘Name’ and ‘Description’ then click ‘Add Virtual Host’ as shown below;

 




Once you have created a virtual host, the result should be as shown below;

 

Systems Frameworks and Hosting Bundle
These tools and components are essential for developing, deploying, and running .NET Core and ASP.NET Core applications. In these sections their use is purposely for running the system in deployment mode.

1.	NET Core SDK:
•	The .NET Core SDK (Software Development Kit) is a set of tools that enables developers to build .NET Core applications. It includes the .NET Core runtime, libraries, command-line tools, and other resources necessary for developing and compiling .NET Core applications. The SDK is required for developing new applications and maintaining existing ones.

2.	.NET Core Runtime:
•	The .NET Core Runtime is the environment required to run .NET Core applications. It includes the .NET Core libraries and runtime components necessary for executing .NET Core code. The runtime is necessary for deploying and running .NET Core applications on a server or other computing environment.

3.	ASP.NET Core Runtime:
•	The ASP.NET Core Runtime is a subset of the .NET Core Runtime specifically tailored for running ASP.NET Core web applications. It includes additional components and libraries necessary for hosting and serving ASP.NET Core web applications, such as the Kestrel web server.

4.	Hosting Bundle:
•	The Hosting Bundle is a package that includes the .NET Core Runtime, ASP.NET Core Runtime, and additional dependencies required for hosting and running .NET Core or ASP.NET Core applications on a server or other production environment. It simplifies the deployment process by bundling all necessary components into a single package.

In the deployment environment, ensure you install the following SDKs, .NET Core frameworks, and the hosting bundle as listed below.
•	Microsoft .NET SDK 8.0 
•	Microsoft .NET SDK 6.0
•	Microsoft ASP.NET Core 6.0
•	Microsoft ASP.NET Core 8.0
•	Microsoft .NET Runtime – 6.0
•	Microsoft .NET Runtime – 8.0
•	Microsoft .NET 8.0.1 Windows Server Hosting




Systems Installation
For a new installation, prepare the Vanguard Financials appropriate directories. These determine where different services and Vanguard Financials resources can be located by sub-systems and users for normal operations and diagnostics.

Running Vanguard Financials recommends splitting the main server / Widows PC disk into more than one partitions. 
This enhances: 
	Disk optimization to improve efficiency
	Improved Performance
	Reduce risks of data loss when 1 partition fails
	Selective and Quicker backups and restores 
	Granular Recovery options 




Automated File Creation
Use the automating tool to create the necessary folders.
Proceed and run vfin_DirSort script to generate all the folders and sub-folders accordingly.
 
Incase the tool is under maintenance or does not work, go ahead and create ‘VanguardFinancials’ folder in your Secondary drive e.g. 
D:\VanguardFinancials or E:\VanguardFinancials.
Then proceed and create the various subfolders as shown in the image below;


