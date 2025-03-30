<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines using Wireshark</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, DHCP, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (22H2)
- Ubuntu Server (22.04)

<h2>High-Level Steps</h2>

- Create a Resource Group
- Create a Windows 10 & Linux Ubuntu VM
- Download Wireshark
- Observe ICMP Traffic
- Configuring a Firewall (Network Security Group)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic
<br />
<h2>Setup Azure Resources</h2>

<h3>Create a Resource Group</h3>
<p>
<img src="https://i.imgur.com/jKk29xU.png" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Create a Virtual Network</h3>
<p>
<img src="https://i.imgur.com/dIW36H7.png" width="80%" alt="Disk Sanitization Steps"/>
</p>

<h3>Create Windows 10 Pro VM</h3>
<p>
Now create your Windows Virtual Machine.
</p>
<p>
  <img src="https://i.imgur.com/ZKjdzmc.png" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Create Linux Ubuntu VM</h3>
<p>
  Then create the Linux Virtual Machine
</p>
<p>
<img src="https://i.imgur.com/3yJa5Qg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h2>Performing Activities on the Network - Actions and Observations</h2>
<h3>Observe some ICMP traffic</h3>
<p>
  Use Remote Desktop to connect to your Windows 10 Virtual Machine
</p>
<p>
<img src="https://i.imgur.com/mumFoXf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within your Windows 10 Virtual Machine, Download Wireshark in the VM.

  Wireshark Download link: https://www.wireshark.org/download.html 
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
Retrieve the private IP address of the Ubuntu VM (Linux-VM) and attempt to ping it from within the Windows 10 VM. 
  
  (It should work since Ping is ICMP traffic and we filtered Wireshark to inspect ICMP traffic) 
</p>
<p>
  <img src="https://i.imgur.com/mMBHK5V.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
   <img src="https://i.imgur.com/3GdDS6m.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Observe Ping Requests and Replies within Wireshark,
</p>
<p>
  <img src="https://i.imgur.com/GRXNNu6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/saR3keR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  The whole data we inspected was the ICMP Echo Request, meaning it was from Windows VM and the next packet is the Echo Reply from the Linux VM.
</p>
<br />

<h3>Configuring a Firewall (Network Security Group)</h3>
<p>
  Initiate a Perpetual/non-stop Ping from your Windows 10 VM to your Ubuntu VM in PowerShell.
</p>
<p>
  <img src="https://i.imgur.com/LeckwaD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Back in Azure, Open the Network Security Group your Ubuntu VM is using and disable incoming 
  
(inbound) ICMP traffic.

To disable ICMP Traffic, we would create specific security rules to block inbound ICMP traffic in an Azure NSG, which would look like this:

- Direction: Inbound

- Protocol: ICMP

- Source: Any (or specify a specific source)

- Destination: Any (or specify a specific destination)

- Action: Deny
</p>
<p>
  <img src="https://i.imgur.com/hYg1GQO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/O7aH0nP.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  This rule would prevent any ICMP traffic (such as ping requests) from being received by your Ubuntu VM or resource.
  
  Hop back in the Windows 10 VM, Observe the ICMP traffic in Wireshark and the command line Ping activity.
</p>
<p>
<img src="https://i.imgur.com/ErvCS4U.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/wxZzEY3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
We Configured a Rule that Denied Incoming ICMP traffic from any source to any destination for the Linux VM.

  Now Re-enable the ICMP traffic for the Network Security Group your Ubuntu VM by Deleting the Network Security rule. 
</p>
<p>
  <img src="https://i.imgur.com/1z6ICuf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity (should start working).
</p>
<p>
  <img src="https://i.imgur.com/Cjp3y8A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Stop the ping activity with "Control-C".
</p>
<p>
  <img src="https://i.imgur.com/8sLQVcT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Observe SSH Traffic</h3>
<p>
  Log back into the Windows-VM, go back in Wireshark and start a packet capture up.

  Filter for SSH traffic only - (Secure Shell is used to make a secure connection from one computer to another, SSH can be used to connect to it and administer that device, SSH uses TCP port 22) 
</p>
<p>
  <img src="https://i.imgur.com/bkpT8wD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From your Windows 10 VM, “SSH into” your Linux Ubuntu Virtual Machine (via its Private IP address) – We’ll connect into the Ubuntu Virtual Machine using SSH.

  Open PowerShell, and type: ssh labuser@private ip address - "ssh labuser@10.0.0.5"

  Then Type the commands (username, pwd, etc) into the Linux SSH connection and observe SSH traffic spam in Wireshark 
</p>
<p>
  <img src="https://i.imgur.com/mupELhr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/ig8lFEA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/PTuVVvC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  What's cool about SSH is all the Traffic is Encrypted.
</p>
<p>
  <img src="https://i.imgur.com/ZHmGyAW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/F9Mkjk6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/B0JGJJZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Exit the SSH connection by typing ‘exit’ and pressing [Enter]
</p>
<p>
  <img src="https://i.imgur.com/GzpIg4I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Observe DHCP Traffic</h3>
<p>
  Back in Wireshark, filter for DHCP traffic only 
</p>
<p>
   <img src="https://i.imgur.com/jLx2QAw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From your Windows 10 VM, attempt to issue your VM a new IP address from the command line 

  Open PowerShell as Admin and run: ipconfig /renew 
</p>
<p>
  <img src="https://i.imgur.com/wBIJaj5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Trying this gave me an Error!, 

  Let's try this method and Observe the DHCP traffic appearing in Wireshark.

  Here's a Script to Run in PowerShell as Admin.
  
  Open Notepad, Type
  
  - ipconfig /release
  - ipconfig /renew

  Save the Script in ProgramData as a file with a ".bat" extension (Not ".txt"). For convenience, you could name the file "dhcp.bat."

  Go to PowerShell as Admin, cd (Change Directory) to "c:\programdata"

  Then Run the File, ".\dhcp.bat"

  Note the VM will loose connection but will automatically renew.
</p>
<p>
  <img src="https://i.imgur.com/AyhzyA5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/6omwHYO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>Observe DNS Traffic</h3>
<p>
  Back in Wireshark, filter for DNS traffic only 
</p>
<p>
  <img src="https://i.imgur.com/GZrualt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From your Windows 10 VM within a command line, use nslookup to see what disney.com and pixar.com’s IP addresses are.

  Observe the DNS traffic being shown in Wireshark 
</p>
<p>
   <img src="https://i.imgur.com/0lvCDgF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/QYc9QXY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  All DNS does is resolve host (Human Readable) names into IP Addresses.
</p>
<br />

<h3>Observe RDP Traffic</h3>
<p>
  Back in Wireshark, filter for RDP traffic only (tcp.port == 3389) 
</p>
<p>
  <img src="https://i.imgur.com/Ctc83Zi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Observe the immediate non-stop spam of traffic!
</p>
<p>
  <img src="https://i.imgur.com/TFDN3L4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?

  It's because the RDP (protocol) is constantly showing you a live stream from one computer/server to another, therefore the traffic is always being transmitted.
</p>
<br />
<p>
  Now that we're finished observing the Network, Don't forget to clean up your Azure Environment! This will prevent you from unnecessary incurring additional charges. 
</p>
