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

<p>Access the Windows 10 Virtual Machine using Remote Desktop. Once inside, install and launch <a href="https://www.wireshark.org" target="_blank">Wireshark</a>. Start a packet capture and filter for ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/e6213f68-f090-4dc7-8680-b0289923f4a7)

![image](https://github.com/user-attachments/assets/982705c6-3f53-4c32-a568-c69535474264)

![image](https://github.com/user-attachments/assets/1c932529-29e1-491e-a385-e067a55c3b1d)

![image](https://github.com/user-attachments/assets/803f4f12-095a-431b-87a0-d019e46c4fa1)

![image](https://github.com/user-attachments/assets/9d3d5c3a-b382-4992-b9d8-60353c15e331)

<br />

<p>Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM using <strong>Powershell</strong>, observing the requests and replies in Wireshark.</p>

![image](https://github.com/user-attachments/assets/a0e5c342-99ec-47c3-9000-20d26e4e36c8)

![image](https://github.com/user-attachments/assets/6309a736-101b-41ed-9aa1-9d9b9dcc732d)

![image](https://github.com/user-attachments/assets/8b06dc39-3701-494c-a25a-f74440e31db9)

<br />
<br />

<h3>Part 3: Configuring a Firewall (Network Security Group)</h3>

<p>Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM.</p>

![image](https://github.com/user-attachments/assets/ca4accbc-d69c-45e2-8235-36559584f6a7)

<p>Navigate to the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/d8701a00-f06d-4a05-8343-38eddcd53794)

![image](https://github.com/user-attachments/assets/e14b551b-b53b-4dd0-be7b-802024b18409)

![image](https://github.com/user-attachments/assets/3830e240-e2b3-489f-b95f-aad3cd754a1b)

![image](https://github.com/user-attachments/assets/14ddb7a0-b118-46c2-b37b-e527ab21fe35)



<p>Back in the Windows VM, observe how the ping fails and how this change is reflected in Wireshark.</p>

![image](https://github.com/user-attachments/assets/97f89e34-fdb9-42a6-8b2f-faa1a4e72646)

  
<p>Re-enable ICMP traffic in the Linux VM Network Security Group and confirm that the ping resumes successfully in both the command line and Wireshark.</p>

![image](https://github.com/user-attachments/assets/42771371-30a3-4e38-b310-17fb64bb3070)

![image](https://github.com/user-attachments/assets/b5df7366-ea7f-41f4-b62d-5fde5070ad3f)

![image](https://github.com/user-attachments/assets/7eb63030-9248-41a3-b823-90684f7ea88c)

<br />

<h3>Part 4: Observing Traffic Between Azure Virtual Machines</h3>

<p>Next, start a new Wireshark packet capture and filter for SSH traffic.</p>

![image](https://github.com/user-attachments/assets/a9b10436-3c82-4157-b69d-bc3024e33052)

![image](https://github.com/user-attachments/assets/75aa58c9-e91f-423c-b213-89c52f6ee953)

 
<p>From the Windows VM, initiate an SSH connection to the Ubuntu VM using its private IP address. Enter the necessary login credentials and observe the SSH traffic in Wireshark. After testing, exit the SSH session.</p>

![image](https://github.com/user-attachments/assets/eef81b1e-454d-4638-8e5b-5d2ba63c27ca)

![image](https://github.com/user-attachments/assets/c7450ad9-121e-4151-9847-8f9362b8c814)

![image](https://github.com/user-attachments/assets/ab63265c-af13-4f4c-9d95-021531786491)

<p>Similarly, filter for DHCP traffic and renew the Windows VM’s IP address using the "ipconfig /renew" command in PowerShell, watching the DHCP requests and responses in Wireshark.</p> 

![image](https://github.com/user-attachments/assets/7da8de65-22f9-428c-8936-c047aaca8512)

![image](https://github.com/user-attachments/assets/d37cf2a6-d28a-4564-bea3-93d4cd2c37b9)

![image](https://github.com/user-attachments/assets/dc49d446-a1a4-43cd-aafe-b32b77125a55)


<p>Then, filter for DNS traffic and use the "nslookup" command to query domain names like "google.com" and "disney.com," observing the DNS requests and responses in Wireshark.</p>

![image](https://github.com/user-attachments/assets/9f6bdc66-b400-4458-b597-fe8fb3712714)

![image](https://github.com/user-attachments/assets/3a957ec9-e7c2-4ac2-a1c1-8a0ebe72a96a)


<p>Finally, filter for RDP traffic (tcp.port == 3389) and note the continuous data flow, as the protocol constantly transmits display updates between machines.</p>


<br />

<hr>

<h3>Lab Cleanup</h3>

<p>Close the Remote Desktop connection to the Windows VM. In the Azure portal, delete the Resource Group that was created at the beginning of the lab. Ensure that all associated resources are removed by verifying the Resource Group deletion.</p>


