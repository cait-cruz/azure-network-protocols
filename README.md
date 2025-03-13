<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />




<h2>Environments and Technologies Used</h2>

- <strong>Cloud Provider:</strong> Microsoft Azure
- <strong>Tools:</strong> Wireshark, Remote Desktop, PowerShell, Command Prompt
- <strong>Network Services:</strong> Virtual Network (VNet), Subnet, Network Security Group (NSG)
- <strong>Various Network Protocols:</strong> SSH, RDP, DNS, ICMP, DHCP




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

<p>
Log into the Azure Portal and navigate to "Resource Groups." Click "Create," enter a name and region, then select "Review + Create" to finalize the resource group. 
</p>

![image](https://github.com/user-attachments/assets/8a964873-724f-4f7e-9444-425301f9df94)
<br />

<p>
Next, create a Windows 10 Virtual Machine by going to "Virtual Machines," clicking "Create," and selecting "Windows 10 Pro, Version 22H2," with the size of "Standard_D2s_v3 - 2vcpus." Assign the previously created Resource Group and create a new Virtual Network (Vnet) and Subnet. Set up authentication using a username and password. 
</p>

![image](https://github.com/user-attachments/assets/6aba1333-8288-4515-a1fa-29a881f1b089)

![image](https://github.com/user-attachments/assets/1b2686e3-1087-4fbb-969b-2961fac48f44)

![image](https://github.com/user-attachments/assets/feb02eb5-82e5-4e2e-923f-8238116096f0)

![image](https://github.com/user-attachments/assets/96441ee6-f630-4ce0-9bcc-32e45b270315)

<br />
<p>
Then, create a Linux (Ubuntu Server 22.04 LTS - x64 Gen2) Virtual Machine, ensuring it uses the same Resource Group and Virtual Network as the Windows 10 VM. Set <strong>Authentication type</strong> to <strong>Password.</strong> Confirm that both VMs are within the same Virtual Network and Subnet and create.
</p>

![image](https://github.com/user-attachments/assets/93b3a303-a729-4c83-bd46-0456b29675d6)

![image](https://github.com/user-attachments/assets/3d4e522c-d6a0-4934-9990-4a11f07c7332)

<br />

<h3>Part 2: Observing ICMP Traffic</h3>

<p>Access the Windows 10 Virtual Machine using Remote Desktop. Once inside, install and launch Wireshark. Start a packet capture and filter for ICMP traffic.</p>


<br />

<p>Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM, observing the requests and replies in Wireshark.</p>


<br />
<br />

<h3>Part 3: Configuring a Firewall (Network Security Group)</h3>

<p>Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM.</p>



<p>Navigate to the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.</p>



<p>Back in the Windows VM, observe how the ping fails and how this change is reflected in Wireshark.</p>


  
<p>Re-enable ICMP traffic in the Linux VM Network Security Group and confirm that the ping resumes successfully in both the command line and Wireshark.</p>


<br />

<h3>Part 4: Observing Traffic Between Azure Virtual Machines</h3>

<p>Next, start a new Wireshark packet capture and filter for SSH traffic.</p>


 
<p>From the Windows VM, initiate an SSH connection to the Ubuntu VM using its private IP address. Enter the necessary login credentials and observe the SSH traffic in Wireshark. After testing, exit the SSH session.</p>



<p>Similarly, filter for DHCP traffic and renew the Windows VM’s IP address using the "ipconfig /renew" command in PowerShell, watching the DHCP requests and responses in Wireshark.</p> 



<p>Then, filter for DNS traffic and use the "nslookup" command to query domain names like "google.com" and "disney.com," observing the DNS requests and responses in Wireshark.</p>



<p>Finally, filter for RDP traffic (tcp.port == 3389) and note the continuous data flow, as the protocol constantly transmits display updates between machines.</p>


<br />

<hr>

<h3>Lab Cleanup</h3>

<p>Close the Remote Desktop connection to the Windows VM. In the Azure portal, delete the Resource Group that was created at the beginning of the lab. Ensure that all associated resources are removed by verifying the Resource Group deletion.</p>


