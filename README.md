<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (21H2)
- Ubuntu Server 22.04

<h2>High-Level Steps</h2>

- Create a Resource Group
- Create a Windows and Linux Virtual Machine
- Download Wireshark
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>

<h3>Create a Resource Group</h3>
<p>
<img src="https://i.imgur.com/jKk29xU.png" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Create a Virtual Network</h3>
<p>
<img src="https://i.imgur.com/dIW36H7.png" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Create Windows 10 VM</h3>
<p>
Now create your Windows Virtual Machine.
</p>
<p>
  <img src="https://i.imgur.com/ZKjdzmc.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Create Linux VM</h3>
<p>
  Then create the Linux Virtual Machine
</p>
<p>
<img src="https://i.imgur.com/3yJa5Qg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<h2>Performing Activities on the Network</h2>
<h3>Observe some ICMP traffic</h3>
<p>
  Use Remote Desktop to connect to your Windows 10 Virtual Machine
</p>
<p>
<img src="https://i.imgur.com/mumFoXf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within your Windows 10 Virtual Machine, Download Wireshark on VM.
</p>
<p>
  <img src="https://i.imgur.com/sA2E4sp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Wireshark and start packet capture, Inspect Traffic.
</p>
<p>
  <img src="https://i.imgur.com/ZbrwHdI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within Wireshark, filter for ICMP traffic only - This is what Ping uses to test connectivity between two devices. 
</p>
<p>
  <img src="https://i.imgur.com/w96ivrp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM. 
  
  (It should work since Ping is ICMP traffic and we filtered Wireshark to inspect ICMP traffic) 
</p>
<p>
  <img src="https://i.imgur.com/mMBHK5V.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   <img src="https://i.imgur.com/3GdDS6m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Observe ping requests and replies within WireShark,
</p>
<p>
  <img src="https://i.imgur.com/GRXNNu6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/saR3keR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  The whole data we inspected was the ICMP Echo Request, meaning it was from Windows VM and the next packet is the Echo Reply from the Linux VM.
</p>
<br />
