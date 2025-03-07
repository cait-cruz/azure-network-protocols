<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- Cloud Provider: Microsoft Azure
- Tools: Wireshark, Remote Desktop, PowerShell, Command Prompt
- Network Services: Virtual Network (VNet), Subnet, Network Security Group (NSG)
- Various Network Protocols: SSH, RDP, DNS, HTTP/S, ICMP, DHCP


<h2>Operating Systems Used </h2>

- Windows 10 (21H2) (Virtual Machine on Azure)
- Ubuntu Server 20.04 (Virtual Machine on Azure)

<h2>High-Level Steps</h2>

- Create Azure Virtual Machines (Windows 10 and Ubuntu) within the same Virtual Network.
- Observe network traffic using Wireshark while testing ICMP, SSH, DHCP, DNS, and RDP protocols.
- Configure a Network Security Group to block and allow specific network traffic.
- Clean up resources to avoid unnecessary charges.


<h2>Actions and Observations</h2>

<br />
<p>
Log into the Azure Portal and navigate to "Resource Groups." Click "Create," enter a name and region, then select "Review + Create" to finalize the resource group. 
</p>
<br />

![Screenshot 2025-03-06 174844](https://github.com/user-attachments/assets/82781fa1-3cde-45da-89d3-4235089123fa)
<br />

<p>
Next, create a Windows 10 Virtual Machine by going to "Virtual Machines," clicking "Create," and selecting "Windows 10 Pro, Version 22H2," with the size of "Standard_D2s_v3 - 2vcpus." Assign the previously created Resource Group and create a new Virtual Network (Vnet) and Subnet. Set up authentication using a username and password. 
</p>
<br />

![Screenshot 2025-03-06 184220](https://github.com/user-attachments/assets/0a36de63-3e74-44ae-81f6-d2264db4cb46)
<br />

![Screenshot 2025-03-06 184242](https://github.com/user-attachments/assets/7de32a8c-7788-4b83-981c-4f726fe5e3e0)
<br />

![Screenshot 2025-03-06 185036](https://github.com/user-attachments/assets/24520442-fa18-4a3e-8daa-c7364112f0f7)
<br />

![Screenshot 2025-03-06 185705](https://github.com/user-attachments/assets/5475cb14-60cd-4b5c-aaa1-2580fb499a57)
<br />


<p>
Then, create a Linux (Ubuntu) Virtual Machine, ensuring it uses the same Resource Group and Virtual Network as the Windows 10 VM. Configure authentication with the same username and password used for the Windows 10 Virtual Machine, and confirm that both VMs are within the same Virtual Network and Subnet.
</p>
<br />


