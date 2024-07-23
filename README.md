<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Systems to Network with Azure for Active Directory</h1>
This tutorial outlines the implementation of on-premises Active Directory Using Azure Virtual Machines as a template
<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Table of Contents</h2>

- 1. Introduction
- 2. Setup Resources
- 3. Install Active Directory
- 4. Create Accounts

<h2>Introduction</h2>

<p>
Active Directory is a powerful tool, and while this tutorial will focus on using a virtual machine as the server, the structure can be applied to physical machines as well. This tutorial and guide has been made to outline the steps needed to bring a server with Active Directory online, and how to route other computers through said server. There are a few steps that must be taken before Active Directory can be explored, or utilized. With Physical machines the designated server computer must have a compatible OS, like Microsoft Server 2022, installed and ready to go. With Virtual machines, the amount of preptime is reduced, as the server compatible OS can be selected as part of the creation process, without the lengthy installation process.
</p>
<br />

<h2>1. Setup Resources</h2>
<p>
The first step in this process is to create the Virtual machines. This will require at least two instances to be created. One running a server OS, like Microsoft Server 2022, and the other running any consumer grade OS. Ensure that both Virtual Machines are established within the same Virtual Network. This can be Verified with Network Watcher. The Virtual Machine with The server OS installed, from now on referred to as DC-1, should have its NIC Private IP set to "Static". This is important to do, as this is where the rest of the network will route through, and if it is dynamic, other computers in the network will not be able to locate this server.
</p>
<p>
  <img src="https://i.imgur.com/3NO7ePm.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  To do this, Go to network settings and click on the NIC settings. This will open up a page with a new menu bar on the left side. Select "IP Configurations'' under the "Settings" group. There will be a box toward the bottom of the page that contains the status of the IP address, with the default being set to "Dynamic''. Click this box to open a new page.
 <p>
  <img src="https://i.imgur.com/HMBoPzs.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  At the bottom of the list of options will be the option to toggle between "Dynamic'' and "Static", make sure the option for static is selected, and save the changes. DC-1 should be the center of this network, with each machine connected to it, and using DC-1's V-net, and group. To Verify that Connectivity to DC-1 is possible, log into DC-1, via Remote Desktop, and enable ICMPv4 on the local Windows Firewall. This will allow the Client Machines to Ping DC-1. This is not a necessary step, but it is helpful to ensure there is connectivity between Virtual Machines.

  Secondly, turn to the networked machines running consumer OS, from now on referred to as Client. When the Client machines are created, it is vitally important for Active Directory that they share the same network with DC-1. Azure will create a Vnet with the creation of DC-1, so when creating the Client machines, DC-1's Vnet will appear in the Network dropdown menu. Select this network with the creation of each new Client machine. 
</p>
<br />

<h2>Install Active Directory</h2>
<p>
<img src="https://i.imgur.com/cHKfZyn.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now that all the pieces are in place, it is time for the installation process to begin. Using Remote Desktop, log into DC-1. Server Manager should automatically launch. IF it does not, it can be found in the Start Menu. Using the quick start guide, Click on "Add roles and Features". This will bring up a setup wizard. Ensure that DC-1 is correctly selected, and continue. 
</p>
<p>
<img src="https://i.imgur.com/TQrGx9d.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Once you reach this page, Select "Active Directory Domain Services", and on the popup window, select "Add Feature". Allow the default options for the rest of the pages, and click "Next" until you are presented with the option to Install the Feature. The installation will take a moment. Once installed, Close the wizard, and return to the Server Manager Dashboard.
</p>
<p>
<img src="https://i.imgur.com/dsCF2Nx.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
To finish the installation of Active Directory, and to set up the server as a Domain Controller, select the Yellow Triangle that should appear in the upper right hand corner of the dashboard. 
</p>
  <p>
<img src="https://i.imgur.com/4mmrvMi.jpeg" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  This will bring up another wizard. Select "Add a new forest, select a domain name (ie "mydomain.com"), and create a password for recovery and security. Leave the options as default and select "Next" on each subsequent page, until you are prompted to install. From here the wizard will install the rest of the necessary software, and configure the server to be a Domain Controller. At the end of this installation process, the machine will need to restart to reflect the changes made to it. This will end your Remote desktop session. This is normal.
</p>
<p>
<img src="https://i.imgur.com/ZUD2qIU.jpeg" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  When Logging back in to DC-1, you will now have to enter the Domain name, and password. Select "More Options'', and select "Use a Different Account". Enter in the Domain Name that you generated when you created the Forest above, followed by your username (ie, "mydomain.com\username"). The password from previous Remote Desktop logins should remain the same. This initial Login might take some time, but this is normal. 

The next step is to tie the Client machines to DC-1. This involves changing the DNS settings of the client machine to match that of DC-1. Through the Azure portal, navigate to DC-1, to copy the private IP Address from the "Networking" option on the left hand menu. 
</p>
<p>
<img src="https://i.imgur.com/kb7X0ZL.jpeg" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
With this copied, navigate to the client machine, select "Networking” from the left hand menu,  and locate the "Network Interface". Click on the client machine's name located there to open a new window. In this window, navigate to "DNS servers" in the Left hand menu, and click the option. in the new page, select "Custom", and input the copied IP address from DC-1. Save these changes, and restart the client machine.
</p>
<p>
<img src="https://i.imgur.com/I2IUgbK.jpeg" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Now that the client machine is able to see DC-1, the last step is to join the Client to DC-1's Active Directory.Log into the Client machine using an admin account. Right-click the start menu and open the "settings" menu. From here, select "Rename this PC", "Change" and then select "Domain". Enter in the created domain name for DC-1, and click "OK" when prompted, enter the admin name and password. The machine will need to restart. The Client Machine is now Joined to DC-1's Active Directory Network.
</p>
<p>
<img src="https://i.imgur.com/eflcBZf.jpeg" height="40%" width="40%" alt="Disk Sanitization Steps"/>
</p>
<p>
  One final bit of setup is to allow the users of the network to be able to access the client machines remotely. Right-click the Start menu, select "Settings, and then "Remote Desktop". Click on the option to "Select users that can remotely access this PC". Click "Add". This will bring up a small window. In the large text box, type "Domain Users" to select the built-in group. click 'check names" to populate the group, click "OK', and then click "OK", to verify the changes.
</p>
<br />

<h2>Create Accounts</h2>
<p>
<img src="https://i.imgur.com/ruPkjCg.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
To finish setting up Active Directory, and make it usable, admin and user accounts need to be created to give access to the network. This important task is fairly straightforward, and should be done as a continued maintenance task for the health of the network. To open Active Directory, you can access it from the Start Menu by searching for "Active Directory", and selecting "Active Directory Users and Computers", or by selecting it from the "Tools" Menu in the upper right hand corner of the Dashboard.
</p>
<p>
<img src="https://i.imgur.com/6VSUwXc.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
  To maintain organization, and to make future use of this system easier, Creating Organization Units (OU), should be the first step. These OU act similarly to Folders, and allow for grouping of Users by permission type and level. This is done by right-clicking on the domain name you created in this process, selecting "New", and then selecting "Organizational Unit". Select and type a name for the new group and click "OK" (e.g. "Employees'' and "Admin"). You may need to refresh the page to see the new OU. 
</p>
<p>
<img src="https://i.imgur.com/xcLqQaE.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
  To create an account, open the appropriate OU created above, right click, and select "New", and then select "User". When the popup window appears, fill in the details, and select a password. When logging in to this account take note that the full domain name will have to be used along with the admin login (ie "mydomain.com\admin_trust"). Once the user profile has been created, and formatted according to the needs of the organization, the profile is now ready for a user to log in to the network.
  
  To elevate a profile to an admin user, Right click on the user, and select "Properties". In the popup window, select the "Members Of" tab, and click "Add". This will bring up a new popup window. In the large box, type "Domain admin", and click "Check name". This will bring up a window with all the matches for the group that you typed in. Select "Domain Admins" click "OK", click "OK", and then click "Apply". then close the window by clicking "OK".
</p>
<br />
