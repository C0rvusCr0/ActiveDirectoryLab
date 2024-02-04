# ActiveDirectoryLab


<h2>Prerequisites</h2>

I will be using Virtualbox as my hypervisor to create the lab but feel free to use any hypervisor of your choice, the concept is the same. You will need several Windows OS virtual machines. For our purposes, we are using Microsoft evaluation center copies of Windows 10 and Windows Server 2019.


I also assume that the reader is able to do base installations of the above OSs in Virtualbox.
Lab Setup

The lab setup will comprise of the following:

SERVER

    Windows Server 2019: 1 instance

      Processor: 1

      RAM: 2GB

      HDD: 20GB

DESKTOP

    Windows 10: 2 instances

      Processor: 1

      RAM: 2GB

      HDD: 20GB

<h2>Let's start</h2>


Once the Windows Server base operating system is installed I begin setting up the AD that will be called telecorp.local. I shall start off by setting up the network interface of the DC. Log in to the server and open Network and Sharing Center. Setup the IPv4 configuration to look like the following image:

![Fig2](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/e54ca59f-8bc2-4e93-a368-3f2cee9dde5f)


If all is setup properly you should be able to ping google.com from powershell on DC01.telecorp.local.


![Fig3](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/c8eab933-ef09-4b26-b071-c4dbd905ba2c)


With all the network interface of DC01 setup I then install the Active Directory Domain Services role to the Windows Server 2019 so that it can prepare the device to become a domain controller for the telecorp.local domain. Start the Add Roles and Features Wizard and leave everything on default until you get to Server Roles.


![Fig4](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/74a89a76-bff8-42a5-96ba-89781634454e)



With the rest of the settings left as default click on Next until you get to the Confirmation section. Ensure you tick the Restart the destination server automatically if required option and click on Yes.



![Fig5](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/35e396f1-b726-47b2-acb2-f276078008f2)



Click on Install and let the roles get installed on the server.



![Fig6](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/dce00cf1-9bbc-4da2-862d-e8dea55fd187)


Once installation is complete you can close the window. In the Server Manager window at the top right corner there is a notification flag that shows pending tasks that are required to be done on the server. Start with Promote this server to a domain controller. As this is the first domain in the lab I select the Add a new forest section and give it the root domain name of telecorp.local.




![Fig7](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/935fd1ab-fe25-42f7-8b22-a3d2076175e0)



Next, I set the Forest and Domain functional levels to Windows Server 2012 and I set the DSRM password.



![Fig8](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/0f620aa2-46dc-49fb-8400-62a518f690f8)


You can leave the next items as default and click Next till you get to Additional Option where it will populate the NetBIOS name which should be the same as the domain name.



![Fig9](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/c5eac3df-9dd9-457f-9ea6-b25292fa4dd5)


Again, you can leave the rest of the settings as default and click Next till the Prerequisite Check section. If all has gone well, you should get an All prerequisite check passed notification upon which I can click Install to complete the process.



![Fig10](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/c85a0835-1a33-48fb-89ef-10cd52fe00de)


Once installation is complete, I got a message like this:


![Fig11](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/1ef5d54c-bcf2-4208-b6f3-c8a1a1b4ebfc)


Once the device restarts, I have a ready domain environment waiting for me.

<h2>Adding A PC to The Forest Domain</h2>


Next, I set up a PC that will play the victim in the lab that will be called PC01.telecorp.local. I set up the network interface of the PC. Log in to the PC and open Network and Sharing Center. Setup the IPv4 configuration to look like the following image:



![Fig12](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/8a284156-e06a-4e64-bad3-eaca33b001d5)


And ensure that I can ping DC01.telecorp.local from the PC.



![Fig13](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/5d7f6cb2-5260-4d6a-8939-2288443ac579)


With that the PC is ready to be added to the domain. Open File Explorer and right-click on This PC.



![Fig14](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/340517a2-6b7d-4725-8ad5-4422a90e80ba)


In the System window that opens, under Computer Name click on the Change Settings link.



![Fig15](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/52bfbdca-d2bd-4da0-8f0f-dda54db664dd)


In the System Properties window, select Change.



![Fig16](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/eee17f8c-92f0-4833-888f-5160d04673e7)


In the Computer Name/Domain Changes window ensure that the Computer Name is set to PC01 and Member of is set to telecorp.local.


![Fig17](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/dbbc7c46-fda4-4235-b452-55b0420e4f7c)


Authenticate with the administrator account on to the domain to complete the adding of the PC to the domain.



![Fig18](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/b69db110-d5dc-4fe3-ada2-69537639110a)


With that the PC is now added to the domain.


With that the PC is now added to the domain.


This can be verified as well on the Domain Controller.



![Fig20](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/b72b3f57-a6b9-407d-be26-a637a1cc1ddf)


You will be required to restart the PC to complete the joining of the domain. Restart the PC and login using domain credentials.


![Fig21](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/6296ab3b-833a-448b-bc58-f3413c35d778)


The PC is now part of the domain telecorp.local.


<h2>Building In a Vulnerability: AS-REP Roastable Account</h2>


However, for some strange reason (dark one though), it is possible to disable the pre-authentication prerequisite for user accounts. For example in this [article](https://laurentschneider.com/wordpress/2014/01/the-long-long-route-to-kerberos.html), the author states that in order to benefit from SSO on a database hosted on a Unix server, he has to disable the pre-authentication for the user. So, to do this I create a user account using the Active Directory Users & Computers management interface:



![Fig22](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/bc8f14dd-eaad-474c-b5e7-3d6c48dea7fd)


In the new window fill in the details of the new user being created:



![Fig-23](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/3aa735be-ec7a-4036-bf0c-683dc6dbe902)


Give the user a password that exists in the RockYou list so that we can use this for the word list in the attack phase. Disable the User must change password at next logon and enable the Password never expires option.



![Fig24](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/75258c6d-956e-43e9-a493-a76673322c7d)


Once you click Next the user will be created. Click on Finish.



![Fig25](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/c96148b3-dcb4-40a9-9bdd-f1b9996bc13d)


Right click on the newly created user and select Properties.



![Fig26](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/92ca5f7d-4d28-4796-a5aa-e874384b53f4)


In the property window for the user select the Account tab, under the account options, scroll to the bottom and select the Do not require Kerberos authentication option, and click Ok.



![Fig27](https://github.com/C0rvusCr0/ActiveDirectoryLab/assets/122651345/4699bd9c-6772-45a5-a2b2-60ea55247442)


NOTE:

The Impacket GetNPUsers.py can perform the operation of requesting for a TGT for a user that has this setting enabled. Once in possession of the domain controller response KRB_AS_REP, the attacker can try to find out the victim’s clear-text password offline, by using John The Ripper with the krb5tgs mode, or with hashcat.


I highly suggest going through this site to learn about more AD vulnerabilities and attack vectors:

https://adsecurity.org/
