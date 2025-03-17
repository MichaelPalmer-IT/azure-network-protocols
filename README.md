<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

# Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines

In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

## Operating Systems Used

- Windows 10 (21H2)
- Ubuntu Server 22.04

## High-Level Steps

1. Create Virtual Machines in Azure.
2. Observe ICMP traffic between Virtual Machines using Wireshark.
3. Configure a Firewall (Network Security Group) and analyze its impact on network traffic.
4. Observe various protocol traffic (SSH, DHCP, DNS, RDP) using Wireshark.

---

## Part 1: Create Virtual Machines

1. Log in to [Azure Portal](https://portal.azure.com/).
2. **Create a Resource Group**:
   - Navigate to "Resource Groups" and click "Create."
   - Provide a name for your Resource Group and select a region.
   - Click "Review + Create," then "Create."

![image](https://github.com/user-attachments/assets/7e4ba073-6b52-4371-8734-b9e865388810)

3. **Create a Windows 10 Virtual Machine**:
   - Navigate to "Virtual Machines" and click "Create."
   - Select the Resource Group you just created.
   - Configure the Virtual Machine:
     - OS: Windows 10
     - Create username and password
     - Head to the Networking section, then create a new virtual network titled "Lab2-vnet"
   - Complete the setup and deploy the VM.
  
![image](https://github.com/user-attachments/assets/af13d94f-7f8b-4a47-9967-b2a937bc4ec2)

![image](https://github.com/user-attachments/assets/83cfba9b-0744-4db2-a1cf-01965176e5b3)

![image](https://github.com/user-attachments/assets/26e67eb2-04c8-4e51-81c0-fdb325103403)

![image](https://github.com/user-attachments/assets/c5eddae2-e6a7-4a38-979e-444f62858afb)

4. **Create a Linux (Ubuntu) Virtual Machine**:
   - Navigate to "Virtual Machines" and click "Create."
   - Select the same Resource Group and Virtual Network used for the Windows 10 VM.
   - Configure the Virtual Machine:
     - OS: Ubuntu Server 24.04
     - Authentication: Username/Password.
   - Ensure both VMs are in the same Virtual Network and Subnet as the Windows 10 VM.
   - Complete the setup and deploy the VM.

![image](https://github.com/user-attachments/assets/8a13b057-80aa-4441-a3c5-a70f74f83a6b)

![image](https://github.com/user-attachments/assets/c7280b08-22a0-470c-a0a2-666da6792585)

![image](https://github.com/user-attachments/assets/b054649d-5d22-4839-ad22-b4edc104b2c7)

![image](https://github.com/user-attachments/assets/b89d5ba6-0012-4383-b144-3807e937cc4e)

---

## Part 2: Observe ICMP Traffic

1. Use [Microsoft Remote Desktop](https://apps.microsoft.com/store) to connect to your Windows 10 Virtual Machine (if on Mac, install the client first).
2. **Install Wireshark** on the Windows 10 VM:
   - Download and install Wireshark from [https://www.wireshark.org/](https://www.wireshark.org/).
3. Open Wireshark and start a packet capture.
4. Filter for ICMP traffic in Wireshark.
5. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM:
   - Open Command Prompt or PowerShell and run: `ping <Ubuntu VM Private IP>`.
   - Observe the ping requests and replies in Wireshark.
6. From the Windows 10 VM, ping a public website (e.g., `www.google.com`) and observe the ICMP traffic in Wireshark.

![image](https://github.com/user-attachments/assets/c2e43130-979d-4064-8f99-7f038250ba53)

![image](https://github.com/user-attachments/assets/b97f8b56-ff13-4e23-b828-008b9b3950a2)

![image](https://github.com/user-attachments/assets/9cad2ab3-ac3e-4912-9799-71bf6264e5ba)
---

## Part 3: Configure a Firewall (Network Security Group)

### Observe ICMP Traffic with Firewall Changes

1. Initiate a continuous ping from your Windows 10 VM to the Ubuntu VM:
   - Command: `ping <Ubuntu VM Private IP> -t`.
2. Open the Network Security Group associated with the Ubuntu VM.
3. Disable inbound ICMP traffic in the Network Security Group.
4. Observe the ICMP traffic in Wireshark and the command line Ping activity (should stop).
5. Re-enable ICMP traffic in the Network Security Group.
6. Observe the ICMP traffic in Wireshark and the command line Ping activity (should resume).
7. Stop the ping activity.

![image](https://github.com/user-attachments/assets/e806e62a-b46e-47ac-a356-3058aee39f17)

![image](https://github.com/user-attachments/assets/91b5ef93-b739-43c1-8936-76a07cfcc18f)

![image](https://github.com/user-attachments/assets/724199d2-42ea-4be6-9e3a-84e161b7a6ad)

![image](https://github.com/user-attachments/assets/0a1a1b50-622e-417a-9442-e19f6fa588b0)

![image](https://github.com/user-attachments/assets/a5e6ca3c-5107-49c0-92d5-845c0e9e26fc)

![image](https://github.com/user-attachments/assets/6fe0f6f6-961e-4433-ae81-b6c88c389701)

![image](https://github.com/user-attachments/assets/0474771b-6adb-4997-a9ff-0bb963977ed5)

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
 
 
 
 
 
 
 
 
  
  
