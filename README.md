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

<h3>Part 1: Creating Virtual Machines</h3>
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
Then, create a Linux (Ubuntu Server 22.04 LTS - x64 Gen2) Virtual Machine, ensuring it uses the same Resource Group and Virtual Network as the Windows 10 VM. Configure authentication with the same username and password used for the Windows 10 Virtual Machine, and confirm that both VMs are within the same Virtual Network and Subnet.
</p>
<br />

![Screenshot 2025-03-06 190305](https://github.com/user-attachments/assets/00ac9c54-5e44-41c2-9960-71d94b9b7807)
<br />

![Screenshot 2025-03-06 190318](https://github.com/user-attachments/assets/5c3f126d-2ffd-4c10-a47a-0fceb85872f9)
<br />

![Screenshot 2025-03-06 190330](https://github.com/user-attachments/assets/7e7d65d2-986a-4470-be24-87308236771d)
<br />

![Screenshot 2025-03-06 190524](https://github.com/user-attachments/assets/0fb823fa-2ed1-4805-b1f0-d820640683a0)
<br />

<h3>Part 2: Observing ICMP Traffic</h3>
<br/>

<p>Access the Windows 10 Virtual Machine using Remote Desktop. Once inside, install and launch Wireshark. Start a packet capture and filter for ICMP traffic.</p>
<br />





<p>Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM, observing the requests and replies in Wireshark. Then, open the command line or PowerShell on the Windows VM and ping a public website such as www.google.com, monitoring the ICMP traffic within Wireshark.</p>
<br />

<h3>Part 3: Configuring a Firewall (Network Security Group)</h3>
<br />

<p>Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM. Navigate to the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.</p>
<br />

<p>Back in the Windows VM, observe how the ping fails and how this change is reflected in Wireshark. Re-enable ICMP traffic in the Network Security Group and confirm that the ping resumes successfully in both the command line and Wireshark.</p>
<br />

<p>Next, start a new Wireshark packet capture and filter for SSH traffic. From the Windows VM, initiate an SSH connection to the Ubuntu VM using its private IP address. Enter the necessary login credentials and observe the SSH traffic in Wireshark. After testing, exit the SSH session.</p>
<br />

<p>Similarly, filter for DHCP traffic and renew the Windows VM’s IP address using the "ipconfig /renew" command in PowerShell, watching the DHCP requests and responses in Wireshark. Then, filter for DNS traffic and use the "nslookup" command to query domain names like "google.com" and "disney.com," observing the DNS requests and responses in Wireshark. Finally, filter for RDP traffic and note the continuous data flow, as the protocol constantly transmits display updates between machines.</p>
<br />

<h3>Lab Cleanup</h3>
<br />

<p>Close the Remote Desktop connection to the Windows VM. In the Azure portal, delete the Resource Group that was created at the beginning of the lab. Ensure that all associated resources are removed by verifying the Resource Group deletion.</p>


