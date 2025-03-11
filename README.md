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

![image](https://github.com/user-attachments/assets/bd198cca-1c3f-4505-818d-21b74f395102)
<br />

<p>
Next, create a Windows 10 Virtual Machine by going to "Virtual Machines," clicking "Create," and selecting "Windows 10 Pro, Version 22H2," with the size of "Standard_D2s_v3 - 2vcpus." Assign the previously created Resource Group and create a new Virtual Network (Vnet) and Subnet. Set up authentication using a username and password. 
</p>

![image](https://github.com/user-attachments/assets/f0eab0b8-9aff-46a6-93a0-72b4ba2f90aa)
<br />

![Screenshot 2025-03-06 184242](https://github.com/user-attachments/assets/7de32a8c-7788-4b83-981c-4f726fe5e3e0)
<br />

![Screenshot 2025-03-06 185036](https://github.com/user-attachments/assets/24520442-fa18-4a3e-8daa-c7364112f0f7)
<br />

![image](https://github.com/user-attachments/assets/f94434f2-af1c-4d67-8b26-dc10ba0440c7)
<br />

<p>
Then, create a Linux (Ubuntu Server 22.04 LTS - x64 Gen2) Virtual Machine, ensuring it uses the same Resource Group and Virtual Network as the Windows 10 VM. Configure authentication with the same username and password used for the Windows 10 Virtual Machine, and confirm that both VMs are within the same Virtual Network and Subnet.
</p>

![image](https://github.com/user-attachments/assets/b5e56a36-56e3-478b-b1de-ae3207e51da3)
<br />

![Screenshot 2025-03-06 190318](https://github.com/user-attachments/assets/5c3f126d-2ffd-4c10-a47a-0fceb85872f9)
<br />

![Screenshot 2025-03-06 190330](https://github.com/user-attachments/assets/7e7d65d2-986a-4470-be24-87308236771d)
<br />

![image](https://github.com/user-attachments/assets/8f65d48b-d601-4a91-8e57-0084d537d756)
<br />
<br />

<h3>Part 2: Observing ICMP Traffic</h3>

<p>Access the Windows 10 Virtual Machine using Remote Desktop. Once inside, install and launch Wireshark. Start a packet capture and filter for ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/840beca6-1f89-47f9-9896-97dab608f402)
<br />

![image](https://github.com/user-attachments/assets/b14a3766-6c51-4763-b0d8-d25f5946bd97)
<br />

<p>Retrieve the private IP address of the Ubuntu VM and attempt to ping it from the Windows 10 VM, observing the requests and replies in Wireshark.</p>

![image](https://github.com/user-attachments/assets/60c9cf6e-15b8-4301-b154-b9f9cd49db2b)
<br />
<br />

<h3>Part 3: Configuring a Firewall (Network Security Group)</h3>

<p>Initiate a continuous ping from the Windows 10 VM to the Ubuntu VM.</p>

![image](https://github.com/user-attachments/assets/8bef81f5-b645-43d1-888d-1117a8cd51cf)

<p>Navigate to the Network Security Group associated with the Ubuntu VM and disable inbound ICMP traffic.</p>

![image](https://github.com/user-attachments/assets/6e36e4ff-b3e9-4e34-812d-a4403e80957c)

<p>Back in the Windows VM, observe how the ping fails and how this change is reflected in Wireshark.</p>

![image](https://github.com/user-attachments/assets/2b8b8988-ec0b-48c8-8bab-46a2baa30d98)
  
<p>Re-enable ICMP traffic in the Linux VM Network Security Group and confirm that the ping resumes successfully in both the command line and Wireshark.</p>

![image](https://github.com/user-attachments/assets/5198a5d0-daa3-4db0-8615-d6613c8f1ae2)

![image](https://github.com/user-attachments/assets/7aa0e25b-10bf-4fcf-bcf2-5a3199bca2e8)
<br />

<h3>Part 4: Observing Traffic Between Azure Virtual Machines</h3>

<p>Next, start a new Wireshark packet capture and filter for SSH traffic.</p>

![image](https://github.com/user-attachments/assets/623f9a4f-9193-407d-ac3e-8b727334af55)
 
<p>From the Windows VM, initiate an SSH connection to the Ubuntu VM using its private IP address. Enter the necessary login credentials and observe the SSH traffic in Wireshark. After testing, exit the SSH session.</p>

![image](https://github.com/user-attachments/assets/1550dad4-e7cb-43bc-b0d8-bd34a859a786)

![image](https://github.com/user-attachments/assets/7022e319-58c8-4b9d-81ec-8418fbfaf829)

<p>Similarly, filter for DHCP traffic and renew the Windows VM’s IP address using the "ipconfig /renew" command in PowerShell, watching the DHCP requests and responses in Wireshark.</p> 

![image](https://github.com/user-attachments/assets/f46963a5-f2fd-44bc-b04c-f9f87a92c665)

![image](https://github.com/user-attachments/assets/c9a8ae63-ad12-442f-85ba-230c28245963)

<p>Then, filter for DNS traffic and use the "nslookup" command to query domain names like "google.com" and "disney.com," observing the DNS requests and responses in Wireshark.</p>

![image](https://github.com/user-attachments/assets/305d0473-eacd-425c-9b36-ad075c5461d0)

![image](https://github.com/user-attachments/assets/6a617b06-948d-4ba0-a523-a60fa0cc72d9)

<p>Finally, filter for RDP traffic (tcp.port == 3389) and note the continuous data flow, as the protocol constantly transmits display updates between machines.</p>

![image](https://github.com/user-attachments/assets/d665c938-25ef-4139-b8d6-63f579d430a8)
<br />

<hr>

<h3>Lab Cleanup</h3>

<p>Close the Remote Desktop connection to the Windows VM. In the Azure portal, delete the Resource Group that was created at the beginning of the lab. Ensure that all associated resources are removed by verifying the Resource Group deletion.</p>


