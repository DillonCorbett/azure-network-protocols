<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this demonstration, I monitored different types of network traffic with some Azure Virtual Machines and practiced with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Step 1: Create the virtual machines in Microsoft Azure and setup Wireshark so traffic between them can be monitored.
- Step 2: Monitor ICMP traffic between the two virtual machines and create security rules to block it.
- Step 3: Use SSH to connect to VM-2 and monitor the SSH traffic using Wireshark.
- Step 4: Monitor DHCP traffic.
- Step 5: Monitor DNS traffic.
- Step 6: Monitor RDP traffic.

<h2>Actions and Observations</h2>

*<h3>Step 1</h3>*
![image](https://github.com/noles498/azure-network-protocols/assets/143885547/200a2e68-3809-45de-bf2d-c136942712dc)

<p>
Once in Microsoft Azure, begin by navigating to Virtual Machines. Click "Create" to start creating the virtual machines. Name the Resource Group and the Virtual Machine. Select the operating systems, for this project the first one is a Windows machine, and select how many virtual CPU's you would like. Then, create a username and password, check the box at the bottom under "Licensing" and click "Review & Create" at the bottom.
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/c84a1780-b75b-4c85-86f3-389e012fecf5)

<p>
Once the first VM is finished creating begin the second one. When creating this one, make sure that the Resource Group is the same as the first VM. Give this one a name and make it an Linux Ubuntu machine. Choose the amount of virtual CPU's you would like, give it a username and password, and then navigate to the Networking tab.
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/98eb8fce-8658-4e4c-affa-1d686793b26a)

<p>
When the first VM was created it automatically created a virtual network with it. Within this Networking tab, make sure that this second VM is on the same virtual network as the first VM. Once this is done, click "Review and Create".
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/c00b19ce-6692-4178-a44c-7d7af7be0624)

<p>
Now that the VM's are created, use Remote Desktop to log in to the first one. If you navigate to the Virtual Machines in Azure, you can see the IP address's needed for Remote Desktop. Log in with the username and password that were created for the first VM.
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/0d36bea1-ace8-48f9-9b83-5be89ccd11cb)

<p>
From within the Virtual Machine, open the browser and navigate to wireshark.org and download and setup the wireshark application. Just keep all the default settings for wireshark when going through the installation setup. Once setup is complete open up the application, along with the command line within the VM.
</p>
<br />

*<h3>Step 2</h3>*

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/e86825dd-ee6e-4f78-987c-b99e36c524d9)

<p>
Input ICMP into the filter at the top of the wireshark application so that it only shows ICMP traffic. From within the command prompt, ping the private IP address of the second VM that was created. This private address can be found from within Azure by clicking on the VM from within Azure to get all the details on it. The ICMP traffic here can be observed in the wireshark window.
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/17942824-68b1-4be8-8f88-0f32b013c71d)

<p>
Now ping a website, like say www.google.com and observe its traffic. Then perform a non-stop ping between the two virtual machines using ping [IP address] -t. Now from outside the VM navigate to the second VM from within Azure. Navigate to Network Security Groups in Azure and select the one for the second VM. From here navigate to Inbound security rules.
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/942f4a10-9a81-4907-83d2-0c86b264ba74)

<p>
Now from within the Inbound security rules, click the "+Add" button at the top. In the window that pops up, change the Protocol from "Any" to "ICMP" and the Action from "Allow" to "Deny" and click "Add".
</p>
<br />

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/1555af0a-44cf-4cdf-85ba-ac2b9304748e)

<p>
With the new security rule in place, you can now see that the ping requests have begun to time out. As you can see in the wireshark window, ping requests are being made from VM-1 to VM-2, but VM-2 is blocking it and not sending replies anymore. To let the ping requests to go through either change the rule to "Allow" or just delete the security rule that was created and they will begin working again. To stop the constant ping's go ahead and press control + C on the keyboard.
</p>
<br />

*<h3>Step 3</h3>*

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/cef71130-88ae-4643-ba8b-a7cdfcdb737f)

<p>
For this step change the filter in Wireshark from ICMP to SSH. Then open Windows Powershell instead of command line. Using the username and password that was created for VM-2, use the SSH command and powershell to connect VM-2. Type in ssh [username]@[ip address] and press enter. Then type "yes" and press enter. Finally type in the password, note that when typing the password no characters will appear but just type the password and press enter and as long as it is correct it will work. Once logged into VM-2, the SSH traffic can be observed through Wireshark. To exit the connection between the VM's just type exit and press enter.
</p>
<br />

*<h3>Step 4</h3>*

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/330b0e99-eab1-4d3c-99d7-713dfbe840a5)

<p>
Now to observe some DHCP traffic, change the filter in Wireshark from ssh to dhcp. Back in the command prompt, type ipconfig /renew and press enter. This will attempt to issue and new IP address for the VM and communicate with the DHCP server as seen in the Wireshark window.
</p>
<br />

*<h3>Step 5</h3>*

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/b4437f5a-2272-4f9f-976d-6274652b5509)

<p>
To view some DNS traffic, change the filter in Wireshark now to dns. From command prompt, type nslookup www.google.com and press enter. This requests all the IP addresses associated with the www.google.com domain name. From within Wireshark all the dns traffic that occurred during this request can be seen.
</p>
<br />

*<h3>Step 6</h3>*

![image](https://github.com/noles498/azure-network-protocols/assets/143885547/bf72308a-5007-4df0-b3c5-d63618148c95)

<p>
For the final step, change the filter in Wireshark to "tcp.port == 3389" to monitor all the RDP protocol traffic. Since RDP is being use to remote desktop into this VM, there will be a constant flow of traffic here.
</p>






