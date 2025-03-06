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

- Windows 10 (21H2)
- Ubuntu Server 22.04

<h2>High-Level Steps</h2>

- Create Virtual Machines in Azure.
- Observe ICMP traffic between Virtual Machines using Wireshark.
- Configure a Firewall (Network Security Group) and analyze its impact on network traffic.
- Observe various protocol traffic (SSH, DHCP, DNS, RDP) using Wireshark.


<h2>Actions and Observations</h2>


 1.) The first thing we are going to do is create a resource group so we can put both of our virtual machines in. Once we have our resource group made we then want to make our first virtual machine. The first virtual machine we are going to make is a Windows 10 vm. Select the resource you made, and then name the virtual machine VM1. Make sure you select Windows 10 Pro, version 22H as the operating system. As for the size of the machine we are going to want atleast 2 vcpus, and 16 gb of memory. Create a username and password of your choosing, and keep the inbound port rules as the default options.

<p>
<img src="https://imgur.com/WgPD275.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  

<p>
<img src="https://imgur.com/X6ZMTJG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
2.) After this step we are going to click on next until we get to the networking page and it should automatically create a virtual network and subnet for us. 
  

<p>
<img src="https://imgur.com/XzdSPoR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click review and create our VM.
  
  Now that we have created our first VM we are going to go ahead and create our second VM, but this time it will be a Ubuntu Server 20.04 LTS machine. It will be the same process as creating our first machine but instead we are going to switch the SSH public key to password instead. 
  
<p>
<img src="https://imgur.com/0KT3Fmb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
<p>
<img src="https://imgur.com/pyxsHfF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  
  Click next until we get to the networking page again.
  
  The networking should automatically give us the virtual network from VM1 as well as the subnet. 
  
<p>
<img src="https://imgur.com/3fQXRcw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 Click review and create, and it will create our second VM.
 
 2.) Now that we have both virtual machines up and running we are going to connect to our Windows 10 vm using the remote desktop connection app. Once we are connected we are going to go to our browser and download and install Wireshark.
 
 "Wireshark is a free and open-source packet analyzer. It is used for network troubleshooting, analysis, software and communications protocol development, and education." 
 
 3.) Open wireshark and filter for ICMP traffic only.
 
 <p>
<img src="https://imgur.com/RrtChUe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 4.) We are going to want to retrieve the private IP address of our Ubuntu VM and then attempt to ping it from within our Windows 10 VM using wireshark. To ping the private IP address of the Ubuntu machine open CMD or Powershell on the Windows machine and type: ping 10.0.0.5 or whatever the private IP address is for your Ubuntu machine.
 
<p>
<img src="https://imgur.com/zmJzyne.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://imgur.com/pp4eZdK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 In either CMD or Powershell ping www.google.com and observe the traffic in wireshark.
 
5.) We then are going to initiate a non-stop ping from our Windows 10 VM to our Ubuntu VM.
 
6.) Open the Network Security Group of our Ubuntu machine and disable incoming (inbound) ICMP traffic. To disable incoming ICMP traffic click "Add" new rule and copy everything exactly from the picture. Once that is done you can create the rule and it will create automatically and show up as a new rule.
 
 <p>
<img src="https://imgur.com/r3dH3Yy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
<p>
<img src="https://imgur.com/qiSIrsX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
 
 Now that we have disabled incoming ICMP traffic from VM2 if we go back to VM1 you can see the ping request is timing out. 
 
 7.) Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using
Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working)
Stop the ping activity
 
 8.) The next thing we are going to do is Observe SSH Traffic.
 
 ### Observe SSH Traffic

1. In Wireshark, start a new packet capture and filter for SSH traffic.
2. From the Windows 10 VM, SSH into the Ubuntu VM:
   - Command: `ssh <username>@<Ubuntu VM Private IP>`.
   - Enter the password when prompted (the password will not be visible).
3. Type commands within the SSH session and observe the SSH traffic in Wireshark.
4. Exit the SSH session: `exit`.

![image](https://github.com/user-attachments/assets/63217af8-2ba8-4478-aca0-5508ea2a7999)

![image](https://github.com/user-attachments/assets/44384dcd-d02c-4d53-9b12-377c21cf9a9f)

![image](https://github.com/user-attachments/assets/d2684184-25e9-434e-9ad2-eb0cfa692c7a)

### Observe DHCP Traffic

1. In Wireshark, filter for DHCP traffic.
2. From the Windows 10 VM, issue a new IP address:
   - Open PowerShell as admin and run: `ipconfig /renew`.
3. Observe the DHCP traffic in Wireshark.
4. In this case our vm maintains the same IP. If we were to release our IP address (ipconfig /release) then renew it (ipconfig /renew) we would see the complete DHCP cycle in wireshark. 

![image](https://github.com/user-attachments/assets/16a29a2e-8cd5-443d-9d0e-d3da2414b2b3)

**Observing the full DHCP Cycle**
  - Open notepad and type the release and renew commands
![image](https://github.com/user-attachments/assets/6315b81f-9fd1-4460-8600-d7519a83ab7a)

  - Choose a location to save the program. Here we chose c:\program data
  - You can name the file whatever you want but make sure to save it as a .bat file (this turns it into a simple script that we can run)
  - Make sure to change the 'save as type' to all files
![image](https://github.com/user-attachments/assets/221f9dd3-136a-4390-aea8-6a5e2fdf80ad)

  - Change the directory that PowerShell is accessing to the location of the your .bat file by entering 'cd c:\(filelocation)'
  - In this case I will change the directory to c:\programdata
![image](https://github.com/user-attachments/assets/6ce3473b-d04f-482a-a66f-d4872de76b69)

  - Run the DHCP.bat script that was just created by entering '.\dhcp.bat'
  - This program should temporarily disconnect you from the vm because the IPv4 address is being released and renewed
![image](https://github.com/user-attachments/assets/63bf6209-27a1-45e9-abb3-37e82c4e1f32)

  - Observe the Release - Discover - Offer - Request - Acknowledge steps in the DHCP process
![image](https://github.com/user-attachments/assets/856afb10-5691-419a-8851-1a80570e1166)

### Observe DNS Traffic

1. In Wireshark, filter for DNS traffic.
2. From the Windows 10 VM, use `nslookup` to find IP addresses for websites:
   - Example: `nslookup www.bing.com`.
3. Observe the DNS traffic in Wireshark.

![image](https://github.com/user-attachments/assets/d7ed2474-83fa-4ddf-80e3-a6a594531a8e)

### Observe RDP Traffic

1. In Wireshark, filter for RDP traffic:
   - Use the filter: `tcp.port == 3389`.
2. Observe the continuous RDP traffic between the Windows 10 VM and your local machine.

![image](https://github.com/user-attachments/assets/ff04f0f1-eee1-4586-927c-cd2fd44d05da)

---
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
  
