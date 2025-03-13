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

![image](https://github.com/user-attachments/assets/db063212-d225-43fe-a8d7-10f120b47465)


<p>
Next, create a Windows 10 Virtual Machine by going to "Virtual Machines," clicking "Create," and selecting "Windows 10 Pro, Version 22H2," with the size of "Standard_D2s_v3 - 2vcpus." Assign the previously created Resource Group and create a new Virtual Network (Vnet) and Subnet. Set up authentication using a username and password. 
</p>

![image](https://github.com/user-attachments/assets/d31fc699-bed7-418a-8cd7-89dddb257f22)

![image](https://github.com/user-attachments/assets/1b2686e3-1087-4fbb-969b-2961fac48f44)

![image](https://github.com/user-attachments/assets/feb02eb5-82e5-4e2e-923f-8238116096f0)

![image](https://github.com/user-attachments/assets/128d02bd-2caa-4b8b-955e-7383da93d63b)

<p>
Then, create a Linux (Ubuntu Server 22.04 LTS - x64 Gen2) Virtual Machine, ensuring it uses the same Resource Group and Virtual Network as the Windows 10 VM. Set <strong>Authentication type</strong> to <strong>Password.</strong> Confirm that both VMs are within the same Virtual Network and Subnet and create.
</p>

![image](https://github.com/user-attachments/assets/f57ecde8-c1f4-4594-86ab-460c2cb75abe)

![image](https://github.com/user-attachments/assets/ca7e791e-34a6-4fe6-ba45-fffbaf436531)

<h3>Part 2: Observing ICMP Traffic</h3>

<p>Access the Windows 10 Virtual Machine using Remote Desktop. Once inside, install and launch <a href="https://www.wireshark.org" target="_blank">Wireshark</a>. Start a packet capture and filter for ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/e1419d49-9b03-406a-83a7-619a2057fdd7)

![image](https://github.com/user-attachments/assets/7f4b6246-cf5e-45bb-b3df-81e9f3dd48d6)

![image](https://github.com/user-attachments/assets/32a2003d-3271-4284-afa2-09c2d8e1a3ca)

![image](https://github.com/user-attachments/assets/b5024bfc-c58e-4a4d-8309-b43e85e43aa6)

![image](https://github.com/user-attachments/assets/9d3d5c3a-b382-4992-b9d8-60353c15e331)


<p>Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM using <strong>Powershell</strong>, observing the requests and replies in Wireshark.</p>

![image](https://github.com/user-attachments/assets/748ef327-dfc4-4c77-a2e0-695e9faee2be)

![image](https://github.com/user-attachments/assets/2be29573-ae86-4774-89e1-c4439cb55a54)

![image](https://github.com/user-attachments/assets/cf43f1a6-4207-4aa3-bb19-48f2dcb32e83)


<h3>Part 3: Configuring a Firewall (Network Security Group)</h3>

<p>Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM.</p>

![image](https://github.com/user-attachments/assets/fc5480be-4faf-4e66-a11d-758b42edd79a)


<p>Navigate to the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/7534b0f3-9639-4950-860f-c77193719fcb)

![image](https://github.com/user-attachments/assets/f6395bf6-4431-4566-b040-434c2753cb09)

![image](https://github.com/user-attachments/assets/04a064f3-0971-4d06-8853-853579ed99ff)


<p>Back in the Windows VM, observe how the ping fails and how this change is reflected in Wireshark.</p>

![image](https://github.com/user-attachments/assets/97f89e34-fdb9-42a6-8b2f-faa1a4e72646)

  
<p>Re-enable ICMP traffic in the Linux VM Network Security Group and confirm that the ping resumes successfully in both the command line and Wireshark.</p>

![image](https://github.com/user-attachments/assets/3a3084de-77e5-4d13-ad32-8308cae28071)

![image](https://github.com/user-attachments/assets/b5df7366-ea7f-41f4-b62d-5fde5070ad3f)

![image](https://github.com/user-attachments/assets/8ec46078-0404-4a89-94cb-71774707b0f6)



<h3>Part 4: Observing Traffic Between Azure Virtual Machines</h3>

<p>Next, start a new Wireshark packet capture and filter for SSH traffic.</p>

![image](https://github.com/user-attachments/assets/97b6d56d-ecca-4ac9-b89b-dc823c3d1ad8)

![image](https://github.com/user-attachments/assets/75aa58c9-e91f-423c-b213-89c52f6ee953)

 
<p>From the Windows VM, initiate an SSH connection to the Ubuntu VM using its private IP address. Enter the necessary login credentials and observe the SSH traffic in Wireshark. After testing, exit the SSH session.</p>

![image](https://github.com/user-attachments/assets/93004031-3eef-4b18-8cde-f65b315c5fb0)

![image](https://github.com/user-attachments/assets/c7450ad9-121e-4151-9847-8f9362b8c814)

![image](https://github.com/user-attachments/assets/28464560-6fcd-47db-94e5-95ea536e5fca)


<p>Similarly, filter for DHCP traffic and renew the Windows VM’s IP address using the "ipconfig /renew" command in PowerShell, watching the DHCP requests and responses in Wireshark.</p> 

![image](https://github.com/user-attachments/assets/7da8de65-22f9-428c-8936-c047aaca8512)

![image](https://github.com/user-attachments/assets/fed64dfe-aa0e-475d-aa62-ca04a5712034)

![image](https://github.com/user-attachments/assets/eec38df4-0a18-41f7-ae5b-ac11833aa984)


<p>Then, filter for DNS traffic and use the "nslookup" command to query domain names like "google.com" and "disney.com," observing the DNS requests and responses in Wireshark.</p>

![image](https://github.com/user-attachments/assets/9f6bdc66-b400-4458-b597-fe8fb3712714)

![image](https://github.com/user-attachments/assets/5b203745-8b29-4515-9428-87c48763314c)



<p>Finally, filter for RDP traffic (tcp.port == 3389) and note the continuous data flow, as the protocol constantly transmits display updates between machines.</p>

![image](https://github.com/user-attachments/assets/e016148c-6b5d-47a8-a215-7340dacd92e9)

<hr>

<h3>Lab Cleanup</h3>

<p>Close the Remote Desktop connection to the Windows VM. In the Azure portal, delete the Resource Group that was created at the beginning of the lab. Ensure that all associated resources are removed by verifying the Resource Group deletion.</p>


