<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>🌐Network Security Groups (NSGs) and Inspecting Traffic Between Azure VMs using Wireshark</h1>

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with **Wireshark** as well as experiment with Network Security Groups.
<br />

<h2>💻⚙️ Software and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- **Various Network Protocols (SSH, RDP, DNS, DHCP, HTTP/S, ICMP)**
- Wireshark (Protocol Analyzer)

<h2>💻 Operating Systems Used</h2>

- **Windows 10 Pro (22H2)**
- **Linux Ubuntu Server (22.04)**

<h2>Wireshark</h2>

- ### [Download](https://www.wireshark.org/download.html)

<h2>High-Level Steps</h2>

- Create a Resource Group
- Create a Windows 10 & Linux Ubuntu VM
- Download Wireshark
- Observe ICMP Traffic
- Configuring a Firewall (NSGs - Network Security Groups)
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic
<br />

<h2>Setup Azure Resources</h2>

<h3>Create a Resource Group</h3>
<p>
<img src="https://i.imgur.com/jKk29xU.png" width="90%" alt="Disk Sanitization Steps"/>
</p>

<h3>Create a Virtual Network</h3>
<p>
<img src="https://i.imgur.com/dIW36H7.png" width="90%" alt="Disk Sanitization Steps"/>
</p>

<h3>🔷️ Create Windows 10 Pro VM</h3>
<p>
  
Now, create your **Windows 10 Pro** Virtual Machine.
</p>
<p>
  <img src="https://i.imgur.com/ZKjdzmc.png" width="90%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🔷️ Create Linux Ubuntu VM</h3>
<p>
  
  Then, create the **Linux Ubuntu** Virtual Machine.
</p>
<p>
<img src="https://i.imgur.com/3yJa5Qg.png" height="80%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h2>🌐 Performing Activities on the Network - Actions and Observations</h2>
<h3>🔵 Observe ICMP traffic</h3>
<p>
  
  Use **Remote Desktop Connection** to connect to your Windows 10 Pro VM in Azure. 
</p>
<p>
<img src="https://i.imgur.com/mumFoXf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Within your Windows 10 Virtual Machine, Download Wireshark in the VM.

  🔹️**Wireshark Download link**: https://www.wireshark.org/download.html 
</p>
<p>
  <img src="https://i.imgur.com/sA2E4sp.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  After the download, Open Wireshark, start packet capture, and Inspect Traffic.

  Open Wireshark, click "**Ethernet**", then click the blue shark fin at the top left.

  <img src="https://i.imgur.com/m1jJgK5.png" height="90%" width="90%" alt="Disk Sanitization Steps"/>
</p>
<p>
  <img src="https://i.imgur.com/ZbrwHdI.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Next, within Wireshark, filter for "ICMP" traffic only - This is what the "**Ping**" command uses to test connectivity between two devices.

  **Internet Control Message Protocol (ICMP)** is a network layer protocol used by network devices to diagnose network communication or connection issues. 

  - ICMP is mainly used to determine whether or not data is reaching its intended destination promptly. 

  - The ICMP protocol is used on network devices, such as routers, and is crucial for error reporting and testing.
</p>
<p>
  <img src="https://i.imgur.com/w96ivrp.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
Retrieve the Private IP address of the Ubuntu VM (Linux-VM) and attempt to ping it from within the Windows 10 VM. 
  
  (It should work since Ping is ICMP traffic, and we filtered Wireshark to inspect ICMP traffic) 
</p>
<p>
  <img src="https://i.imgur.com/mMBHK5V.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
   <img src="https://i.imgur.com/3GdDS6m.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Observe Ping Requests and Replies within Wireshark,
</p>
<p>
  <img src="https://i.imgur.com/GRXNNu6.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/saR3keR.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>

  You can inspect each packet and view the actual data being sent in each ping.
  
  <img src="https://i.imgur.com/nFirUUB.png" height="100%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  The entire data we inspected consisted of the ICMP Echo Request, which originated from the Windows VM, and the subsequent Echo Reply from the Linux VM.
</p>
<br />

<h3>🔥🧱 Configure a Firewall in Azure (NSGs - Network Security Groups)</h3>
<p>
  Initiate a Perpetual/non-stop Ping from your Windows 10 VM to your Ubuntu VM in PowerShell.
</p>
<p>
  <img src="https://i.imgur.com/LeckwaD.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  Back in Azure, open the Network Security Group your Ubuntu VM is using and **disable** incoming 
  
**(inbound) ICMP traffic**.

To disable ICMP Traffic, we would create specific security rules to block inbound ICMP traffic in an Azure NSG, which would look like this:

- Direction: Inbound

- Protocol: **ICMP**

- Source: Any (or specify a specific source)

- Destination: Any (or specify a specific destination)

- Action: **Deny**
</p>
<p>
  <img src="https://i.imgur.com/hYg1GQO.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/O7aH0nP.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  This rule would prevent any ICMP traffic (such as ping requests) from being received by your Ubuntu VM or resource.
  
  Hop back into the Windows 10 VM, observe the ICMP traffic in Wireshark, and the command line Ping activity.
</p>
<p>
<img src="https://i.imgur.com/ErvCS4U.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>  
<img src="https://i.imgur.com/wxZzEY3.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
We configured a Rule that Denied Incoming ICMP traffic from any source to any destination for the Linux VM.

  Now re-enable the ICMP traffic for the Network Security Group on your Ubuntu VM by deleting the Network Security rule. 
</p>
<p>
  <img src="https://i.imgur.com/1z6ICuf.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command line Ping activity (should start working).
</p>
<p>
  <img src="https://i.imgur.com/Cjp3y8A.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  Stop the ping activity with `Control-C`.
</p>
<p>
  <img src="https://i.imgur.com/8sLQVcT.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🔐 Observe SSH Traffic</h3>
<p>

  Log back into the Windows VM, re-open Wireshark, and initiate a new packet capture.

  Filter for **SSH** traffic only - (**Secure Shell** is used to make a secure connection from one computer to another, SSH can be used to connect to it and administer that device, SSH uses **TCP port 22**)

  For this task, we will use our SSH client on the Windows machine to connect to the Linux machine. SSH has no GUI; it just gives the user access to the machine's CLI (Command-Line Interface). 

  When we use SSH into the Linux Ubuntu machine with the command prompt "ssh labuser@10.0.0.5".
</p>
<p>
  <img src="https://i.imgur.com/bkpT8wD.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  From your Windows 10 VM, let's use the “SSH" protocol into your Linux Ubuntu VM (via its Private IP address) – We’ll connect to the Ubuntu Virtual Machine using SSH.

  Open PowerShell as Administrator, and type: **ssh labuser@private ip address** - "ssh labuser@10.0.0.5"

  Then type the commands (**username**, **pwd**, etc), **replying Yes/No** and clicking [**Enter**] into the Linux SSH connection and observe SSH traffic spam in Wireshark. 
</p>
<p>
  <img src="https://i.imgur.com/mupELhr.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/ig8lFEA.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/PTuVVvC.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  What's cool about SSH is that all the Traffic is **Encrypted**.
</p>
<p>
  <img src="https://i.imgur.com/ZHmGyAW.png" height="90%" width="1000%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/F9Mkjk6.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/B0JGJJZ.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Exit the SSH connection by typing `‘exit’` and pressing [**Enter**]
</p>
<p>
  <img src="https://i.imgur.com/GzpIg4I.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<br />

<h3>🌐 🖥 Observe DHCP Traffic</h3>
<p>
  
  Next, let's use Wireshark to filter for DHCP. **DHCP (Dynamic Host Configuration Protocol)** works on **UDP** ports **67 & 68**. 
  
  The Dynamic Host Configuration Protocol (DHCP) is used to automatically and dynamically assign IP addresses to host machines. 
  
  In this task, we will request a new IP Address with the commands "ipconfig /release" and "ipconfig /renew". Once we enter the commands in PowerShell, Wireshark will capture the DHCP traffic.

  - `ipconfig /release` - (**Disconnect** ❌️ from DHCP Server)
  - `ipconfig /renew` - (**Reconnect** ✅️ to a DHCP Server)

  **Tip**: You can use the `ipconfig` command to help troubleshoot problems with your local DHCP Server.
  
  Back in Wireshark, filter for DHCP traffic only.
</p>
<p>
   <img src="https://i.imgur.com/jLx2QAw.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  From your Windows 10 VM, attempt to issue your VM a new IP address from the command line 

  Open PowerShell as Admin and run: `ipconfig /renew` 
</p>
<p>
  <img src="https://i.imgur.com/wBIJaj5.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Trying this gave me an Error❗, 

  Let's try this method and observe the DHCP traffic appearing in Wireshark.

  Here's a Script to Run in PowerShell as Admin.
  
  Open Notepad, Type
  
  - `ipconfig /release`
  - `ipconfig /renew`

  Save the Script in **ProgramData** as a file with a `.bat` extension (Not❗️ ".txt"). For convenience, you could name the file "*dhcp.bat*".

  Go to PowerShell as Admin, `cd` (**Change Directory**) to `c:\programdata`

  Then run the File "**.\dhcp.bat**"

  (Note the VM will lose connection, but will automatically renew)
</p>
<p>
  <img src="https://i.imgur.com/AyhzyA5.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/6omwHYO.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  Here's a Diagram for the Wireshark capture screenshot above showing how the DHCP Server offers/assigns a new IP Address to my Windows machine.

  Steps the DHCP Server and host machine (**Windows VM**) take in action when requesting an IP Address.

🔹️**DORA Process**

  - Discover
  - Offer
  - Request
  - Acknowledgement✅️
</p>
<p>
  <img src="https://i.imgur.com/pxevHAU.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>

  The DHCP Server still gave the same Private IP Address of "10.0.0.4", which was likely to happen.
</p>
<br />

<h3>🌐 Observe DNS Traffic</h3>
<p>

  Return to Wireshark and filter for DNS traffic only. 
  
  **DNS (Domain Name System)** uses **TCP & UDP port 53**. 
</p>
<p>
  <img src="https://i.imgur.com/GZrualt.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>

  From your Windows 10 VM within PowerShell command line, use `nslookup` to see what *"Disney.com"* and *"Pixar.com's"* IP addresses are.

  - Run in the CLI:
  ```powershell
  nslookup disney.com
  nslookup pixar.com
  ```
  
  The `nslookup` command essentially asks our DNS server what the IP addresses of Disney, Pixar, and Google.

  - You can also use `nslookup` to verify if a DNS Server is running. 

  Observe the DNS traffic being shown in Wireshark 
</p>
<p>
   <img src="https://i.imgur.com/0lvCDgF.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/QYc9QXY.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  All DNS does is resolve host (Human-Readable) names into IP Addresses.
</p>
<br />

<h3>🖥 Observe RDP Traffic</h3>
<p>

  Back in Wireshark, filter for **RDP (Remote Desktop Protocol)** traffic only (**TCP port == 3389**).

**RDP** is a **Microsoft protocol** that allows a user(s) to access and control a Windows computer(s) remotely over a Network.
</p>
<p>
  <img src="https://i.imgur.com/Ctc83Zi.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Observe the immediate non-stop spam of traffic!
</p>
<p>
  <img src="https://i.imgur.com/TFDN3L4.png" height="90%" width="100%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Why do you think it’s non-stop spamming vs only showing traffic when you do an activity?

  It's because the RDP (protocol) is constantly showing you a live stream from one computer/server to another; therefore, the traffic is always being transmitted.
</p>
<br />

<p>
  
  **Conclude**
  
  Now that we're finished observing the Network, don't forget to clean up your Azure Environment! This will prevent you from unnecessarily incurring additional charges. 
</p>
