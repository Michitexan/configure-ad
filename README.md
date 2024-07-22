<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Configuring Systems to Network with Azure for Active Directory</h1>
This tutorial outlines the implementation of on-premises Active Directory Using Asure Virtual Machines as a template
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

<h2>Introducton</h2>

<p>
Active Directory is a powerful tool, and while this tutorial will focus on using a virtual machine as the server, the structure can be applied to physical machines as well. This tutorial and guide has been made to outline the steps needed to bring a server with Active Directoy online, and how to route other computers through said server. There are a few steps that must be taken before Active Directory can be explored, or utilized. With Physical machines the designated server computer must have a compatible OS, like Microsoft Server 2022, installed and ready to go. With Virtual machines, the ammount of preptime is reduced, as the server compatible OS can be selected as part of the creation process, without the lengthy installation process.
</p>
<br />

<h2>1. Setup Resources</h2>
<p>
The first step in this process is to create the Virtual machines. This will require at least two instances to be created. One running a server OS, like Microsoft Server 2022, and the other running any consumer grade OS. Ensure that both Virtual Machines are established within the same Virtual Network. This can be Verified with Network Watcher. The Virtual Machine with The server OS installed, from now on referred to as DC-1, should have its NIC Private IP set to "Static". This is important to do, as this is where the rest of the network will route through, and if it is dynamic, other computers in the network will not be able to locate this server.
</p>
<p>
  <img src="https://i.imgur.com/3NO7ePm.jpeg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  To do this, Go to network settings and click on the NIC setings. This will open up a page with a new menu bar on the left side. Select "IP Configurations" under the "Settings" group. There will be a box toward the bottom of the page that contains the status of the IP address, with the default being set to "Dynamic". Click this box to open a new page.
 <p>
  <img src="https://i.imgur.com/HMBoPzs.jpeg" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p> 
<p>
  At the bottom of the list of options will be the option to toggle between "Dynamic" and "Static" make sure the option for static is selected, and save the changes. DC-1 should be the center of this network, with each machine connected to it, and using DC-1's V-net, and group. To ensure that Connectivity to DC-1 is possible, log into DC-1, via Remote Desktop, and enable ICMPv4 in on the local Windows Firewall.

  Secondly, turn to the the networked machines running consumer OS, from now on referred to as Client. When the Client machienes are created, it is vitally important for Active Directory that they share the same network with DC-1. Azure will create a Vnet with the creation of DC-1, so when creating the Client machienes, DC-1's Vnet will appear in the Network dropdown menu. Select this network with the creation of each new Client machine. 
</p>
<br />

<h2>Install Active Directory</h2>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Create Accounts</h2>
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
