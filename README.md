<p align="center">
<img src="https://imgur.com/3HXimZR.png"/>
</p>

<h1>Creating a Home Lab Environment That Uses Active Directory Within Oracle Virtual Box </h1>
This walk-through outlines the process of creating a home lab environment that incorporates Active Directory. Active Directory will be installed on a Windows 2019 server that will be configured on its own virtual machine using Oracle Virtual Box. Then, a Windows 10 Virtual Box virtual machine will be created and connected to the domain server. After generating users into the Active Directory using a Powershell script, we will be able to log in to any of the generated users' accounts using the Windows 10 machine. <br />


<h2>Environments and Technologies Used</h2>

- Oracle Virtual Box(Virtual Machines)
- Microsoft Active Directory
- Windows 2019 Server
- Windows 10


<h2>Operating Systems Used </h2>

- Windows 10</b> (version 22H2)
- Windows 2019

<p>  
<img src = "https://imgur.com/XwWlhWr.png " height="80%" width="80%" alt="Disk Sanitization Steps"/>

</p>

<p>Overview</p>

<p>
<img src = "https://imgur.com/mCtRKHz.png" " height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
 <p>
1. Go to Virtual Box's website to download the software. Then, you can go to Microsoft's website to download a Windows 10 iso and a Windows Server 2019 iso.
</p>                                                                                                 
                                                                                                     
<p>
<img src= "https://imgur.com/SKRHp7D.png" " height="80%" width="80%" />
</p>

<p>
                                                                                                 
                                                                                                 
                                                                                                 
2. Once Virtual Box is installed, we can now create the virtual machines in Virtual Box for the domain controller and Windows desktop. We will create the domain controller first. To create a new virtual machine, first, click on "New". The, under "Iso image", select the Windows 2019 server iso image that was previosly downloaded. I choose to configure the vm with 4 cpus and  2048 MB of RAM.  Under "Network", I two adapters were added to the virtual machine. The first adapter will be NAT and the second adapter will be a internal one. The NAT adapter will be used to connect the internet and the internal adapter will be used for the internal Virtual Box network. 

</p>
<br />

<p>
<img src="https://imgur.com/xGqRr8p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
3. Once the Windows 2019 virtual machine is created, click on the virtual machine in the Virtual Box interface to start it up. When the popup that states "Select the operating system that you want to install" click on Windows Standard Standard Evaluation (Desktop Experience). Then, under "Which type of installation do you want?", click on Custom: Install Windows only (advanced)". After clicking on "Next", it will take a few minutes for Windows to be installed. 


<p>
<img src="https://imgur.com/xGqRr8p.png
" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


</p>
<br />

<p>
<img src="https://imgur.com/47CBowj.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
4. After the installation is completed, a pop up will appear asking you to create a password for the administer account. After the virtual machine restarts, you will need to press CTRL+ALT+DELETE to open the virtual machine to login. You can do this by navigating to the top left of the screen and clicking on "Input". Then under "Input" click on "Keyboard" and then the button that says "Insert CTRL+ALT+DELETE". 
</p>
<br />

<p>
<img src="https://imgur.com/PSMZPGv.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
5. After you are logged into the virtual machine, we can now change the IP address of the virtual machine. This can be acomplished by going to the bottom right of the screen and clicking on Network. Then click on "Change Adapter Settings". You should see two adters that were created when the virutal machine was initially created. We can now start to determine which adapter is the one with the NAT configuration that can conect to the internet and which one is the other adaptert that should be used for our internal network. The adapter that starts  with 10.x.x.x is the NAT adapter and the adapter with a IP address of 16.254.x.x. is the internal network adapter. The NAT adapter can be renamed to be "_INTERNET_" and the internal network can be renamed "X_internal_X". 

</p>
<br />

<p>
<img src="https://imgur.com/ftWznpL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
6. The IP address of the internal adapter can now be changed. After clicking on the internal adapter, under the Networking tab, click on Internet Protocol Version 4. Under the General tab, click on "Use the Following IP address" and enter the IP address of 172.16.0.1 and the subnet mask of 255.255.255.0. No default gateway will be assinged to the adapter since the domain controller itself will be the gateway since it has both the NAT and internal adapters. Since the domain controller will act as the DNS server, the DNS server address for the domain controller can be its own IP address 172.16.0.1 or the loop-back address foo 127.0.0.1. 

</p>
<br />

<p>
<img src="https://imgur.com/YS2MA3u.png" alt="Disk Sanitization Steps"/>
</p>


7. The next step is to install Active Directory and create a domain. To install Active Directory, navigate to the Server Manager for the Windows 2019 server. Next, click on "Add Roles and Features". Then under "Select Server Roles" click on "Active Directory Domain Services".  Then click on "Install". 
8. 
</p>
<br />

<p>
<img src="https://imgur.com/P1ZcQuW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
8. After Active Directory is installed, we will have to do a Post Deployement Configuration where a new domain will be created for Active Directory.  At the top right of Server Manager Dashboard, click on the flag with the yellow sign and then click on "Promote this server to a domain controller". Then under Deployement Configuration, click on "Add a New Forest" and then type in the domain name. For this lab, the domain name can be mydomain.com. A password can now be created for the domain. After the domain installed, the desktop will be restarted and we can login again with the Administer account.
</p>
<br />

<p>
<img src="https://imgur.com/TTstJ26.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


9. Within Active Directory, a new administer account can now be created. Right-click on the start menu and the navigate to Active Dirctory Users and Computers. Then right-click on mydomain.com and click on New and the Organizational Unit. The organizational unit can be named  _ADMINS. 


</p>
<br />

<p>
<img src="https://imgur.com/e2HbX69.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

10. Right-click on the organizational unit _ADMINS and then click on New. Enter the first and last name of the admin users and then add in the user logon name. To put an admin account in the admin group, click on the _ADMINS_ group and then right-click on the admin user and go to properties. Then, click on Member Of and then click on Add. Type in Domain Admins and then click Check before clicking on Apply and then OK.


<p>
<img src="https://imgur.com/tPMcOrQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

11. You can then sign out and lock back into the Windows Server using the newly created admin account.


<p>
<img src="https://imgur.com/kODyfJQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

12. The NAT/RAS connections can now be installed on the domain controller. This can be done by clicking on "Add Roles and Features" under the Server Manager Dashboard. Then under Select Server Roles, click on the check mark box that states Remote Access and Routing, and then click on Install. On the Server Manager Dashboard, right-click on Tools and then click on Routing and Remote Access. Then, under DC, click on "Configure and Enable Routing and Remote Access." Under configuration, click on NAT.  After selecting next, click on the internet adapter.

-SSH traffic commands to try out: pw,  ls.

<p>
<img src="https://imgur.com/8RzNxG4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

13. Back in Wireshark, filter for DHCP traffic only. Observe the DHCP traffic appearing in Wireshark.

14. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig/renew). Observe the DHCP traffic appearing in Wireshark.



<p>
<img src="https://imgur.com/SuPhxqh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>


15. Back in Wireshark, filter for DNS traffic only


16. From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are



<p>
<img src="https://imgur.com/ym4QKCZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

17.Back in Wireshark, filter for RDP traffic only (tcp.port == 3389)


18. Observe the immediate non-stop spam of traffic. This traffic seems to be nonstop because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted


19. Close your Remote Desktop and delete your resource group and all other resources used in the lab.
