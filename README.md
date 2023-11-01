<p align="center">
<img src="https://imgur.com/zenBk6l.png"/>
</p>

<h1>Creating a Home Lab Environment That Utilizes Active Directory Within Oracle Virtual Box </h1>
This walk-through outlines the process of creating a home lab environment that incorporates Active Directory. Active Directory will be installed on a Windows 2019 server that will be configured on its own virtual machine using Oracle Virtual Box. Then, a Windows 10 Virtual Box virtual machine will be created and connected to the domain server. After generating users into the Active Directory using a Powershell script, we will be able to log in to any of the generated users' accounts using the Windows 10 machine. <br />


<h2>Environments and Technologies Used</h2>

- Oracle Virtual Box(Virtual Machines)
- Microsoft Active Directory
- Windows 2019 Server
- Windows 10


<h2>Operating Systems Used </h2>

- Windows 10</b> (version 22H2)
- Windows 2019


<p>Overview</p>

<p>
<img src = "https://imgur.com/b5uWqTP.png" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 <p>
1. Go to Virtual Box's website to download the software. Then, you can go to Microsoft's website to download a Windows 10 iso and a Windows Server 2019 iso.
</p>                                                                                                 
                                                                                                     
<p>
<img src= "https://imgur.com/IC8UvWu.png" " height="80%" width="80%" />
</p>

<p>
                                                                                                 
                                                                                                 
                                                                                                 
2. Once Virtual Box is installed, we can now create the virtual machines in Virtual Box for the domain controller and Windows desktop. We will create the domain controller first. To create a new virtual machine, first, click on "New". Then, under "Iso image", select the Windows 2019 server iso image that was previously downloaded. I chose to configure the VM with 4 CPUs and  2048 MB of RAM.  Under "Network", two adapters were added to the virtual machine. The first adapter will be NAT and the second adapter will be an internal one. The NAT adapter will be used to connect the internet and the internal adapter will be used for the internal Virtual Box network. 

</p>
<br />


<p>
3. Once the Windows 2019 virtual machine is created, click on the virtual machine in the Virtual Box interface to start it up. When the popup that states "Select the operating system that you want to install" click on Windows Standard Standard Evaluation (Desktop Experience). Then, under "Which type of installation do you want?", click on Custom: Install Windows only (advanced)". After clicking on "Next", it will take a few minutes for Windows to be installed. 


<p>
<img src="https://imgur.com/xGqRr8p.png
" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


</p>
<br />

<p>
<img src="https://imgur.com/bK63aQh.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
4. After the installation is completed, a pop-up will appear asking you to create a password for the administrator account. After the virtual machine restarts, you will need to press CTRL+ALT+DELETE to open the virtual machine to log in. You can do this by navigating to the top left of the screen and clicking on "Input". Then under "Input" click on "Keyboard" and then the button that says "Insert CTRL+ALT+DELETE". 
</p>
<br />

<p>
<img src="https://imgur.com/yk4E6vn.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
5. After you are logged into the virtual machine, we can now change the IP address of the virtual machine. This can be accomplished by going to the bottom right of the screen and clicking on Network. Then click on "Change Adapter Settings". You should see two adapters that were created when the virtual machine was initially created. We can now start to determine which adapter is the one with the NAT configuration that can connect to the internet and which one is the other adapter that should be used for our internal network. The adapter that starts  with 10.x.x.x is the NAT adapter and the adapter with an IP address of 16.254.x.x. is the internal network adapter. The NAT adapter can be renamed to "_INTERNET_" and the internal network can be renamed "X_internal_X". 

</p>
<br />

<p>
<img src="https://imgur.com/d8Jk4xY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
6. The IP address of the internal adapter can now be changed. After clicking on the internal adapter, under the Networking tab, click on Internet Protocol Version 4. Under the General tab, click on "Use the Following IP address" and enter the IP address of 172.16.0.1 and the subnet mask of 255.255.255.0. No default gateway will be assigned to the adapter since the domain controller itself will be the gateway since it has both the NAT and internal adapters. Since the domain controller will act as the DNS server, the DNS server address for the domain controller can be its own IP address 172.16.0.1 or the loop-back address for 127.0.0.1. 

</p>
<br />

<p>
<img src="https://imgur.com/tsIvoik.png"/>
</p>


7. The next step is to install Active Directory and create a domain. To install Active Directory, navigate to the Server Manager for the Windows 2019 server. Next, click on "Add Roles and Features". Then under "Select Server Roles" click on "Active Directory Domain Services".  Then click on "Install". 

</p>
<br />

<p>
8. After Active Directory is installed, we will have to do a Post Deployment Configuration where a new domain will be created for Active Directory.  At the top right of the Server Manager Dashboard, click on the flag with the yellow sign and then click on "Promote this server to a domain controller". Then under Deployment Configuration, click on "Add a New Forest" and then type in the domain name. For this lab, the domain name can be mydomain.com. A password can now be created for the domain. After the domain is installed, the desktop will be restarted and we can log in again with the Administer account.
</p>
<br />

<p>
<img src="https://imgur.com/5Hvacts.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


9. Within Active Directory, a new administrator account can now be created. Right-click on the start menu and navigate to Active Directory Users and Computers. Then right-click on mydomain.com and click on New and the Organizational Unit. The organizational unit can be named  _ADMINS. 


</p>
<br />

<p>
<img src="https://imgur.com/59MSC6p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

10. Right-click on the organizational unit _ADMINS and then click on New. Enter the first and last name of the admin users and then add in the user logon name. To put an admin account in the admin group, click on the _ADMINS_ group and then right-click on the admin user and go to properties. Then, click on Member Of and then click on Add. Type in Domain Admins and then click Check before clicking on Apply and then OK.



11. You can then sign out and lock back into the Windows Server using the newly created admin account.


<p>
<img src="https://imgur.com/uO6fkLv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

12. The NAT/RAS connections can now be installed on the domain controller. This can be done by clicking on "Add Roles and Features" under the Server Manager Dashboard. Then under Select Server Roles, click on the check mark box that states Remote Access and Routing, and then click on Install. On the Server Manager Dashboard, right-click on Tools and then click on Routing and Remote Access. Then, under DC, click on "Configure and Enable Routing and Remote Access." Under configuration, click on NAT.  After selecting next, click on the internet adapter.


<p>
<img src="https://imgur.com/1b4whVD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

13. Now, we can set up the DHCP server so that the Windows 10 client can get an IP address from the Windows 2019 server so that they can use the internet. This can be done by clicking on Add Roles on the Sever Management Dashboard and then selecting the DHCP checkbox. Then after selecting Next, click on Install. 

14. The scope for the DHCP server can now be set up. This can be done by first navigating to the DHCP server under the Server Management Dashboard.  Right-click on IPv4 and then click on New Scope. The start address of the DHCP server is 172.16.0.100 and the end IP address is 172.16.0.200. The subnet mask is 255.255.255.0. When asked if you want to configure DHCP options, click on "Yes I want to configure these options now". Under the Router (Default Gateway) option, enter the IP address of the domain controller. After the DHCP scope is created, right-click the DHCP server click on Authorize, and then click on Refresh. 



<p>
<img src="https://imgur.com/x2743rx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


<p>
<img src="https://imgur.com/kprHHRe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

15. We will now download a list of fake users onto Active Directory using a Powershell script. Before running the Powershell script, type Set-Execution Policy Unrestricted. Based on the Powershell script, all of the created users will have a password of Password1. The usernames of the users will have a format of being the first letter of their first name and then their last name. Once the script is run, you should see users starting to be created in blue text. When you navigate to Active Directory Users and Computers and click on the organizational unit labeled _USERS, you can view the list of users being created.

<p>
<img src="https://imgur.com/vU8nxld.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

16. Now we can move on to configuring the Windows 10 virtual machine. Under the Network tab, configure the Windows 10 virtual machine to use an internal network. When configuring Windows 10, make sure to use Windows 10 Pro and not Windows 10 Home since you are unable to connect to a domain using Windows 10 Home.
  
<p>
<img src="https://imgur.com/HBacx7G.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


17. Once Windows 10 is set up, run ipconfig in the command line. If there is no listed default gateway, make sure that routing and remote access are enabled in the DHCP server. Then click on "Change". You can name the computer CLIENT1 and make it a member of mydomain.com.



<p>
<img src="https://imgur.com/Y6G3XGW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
<img src="https://imgur.com/f0YrY9D.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

18. Now, we can finally join the Windows 10 desktop to the Windows 2019 server. To do this, on the Windows 10 Desktop, right-click the Start menu and then click on System. Then click on Rename this PC (advanced). After you connect to the domain named mydomain.com, the Windows 10 desktop can now be logged into using any of the user account information that was created from the Powershell script.



