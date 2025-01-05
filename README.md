![265865191-f64f4a5b-bc43-4cd8-acfd-20bc707d00aa](https://github.com/CollinsU99/Azure-network-protocols/assets/124742607/ca25afdd-b194-426a-b247-356402906ce1)

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Microsoft Azure Virtual Machines</h1>

This lab will focus on using Wireshark to examine network traffic flowing between Azure Virtual Machines (VMs). We will also explore the functionality of Network Security Groups (NSGs).

In simpler terms, this lab will teach you how to use Wireshark to monitor network traffic between Azure VMs and how to use NSGs to control access to Azure VMs.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 Pro (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resource Group
- Create Virtual Machine in Windows and Linux
- Connect to both VMs using RDP (Remote Destop Protocol)
- Download Wireshark on VM1 (Windows 10 Pro)
- Initiate a perpetual/non-stop ping from VM1 to VM2
- Create a Network Security Group in Azure to deny the perpetual/non-stop ping
- Observe SSH,DNS,RDP, and ICMP traffic in Wireshark
- Log into VM2 using SSH
- Run Powershell Commands in VM2
- Exit VM1
- Delete Resource Groups in Azure for both VMs

<h2>Actions and Observations</h2>

<p align="center">
<img src="https://i.imgur.com/EL2Oexz.png" height="80%" width="80%" alt="img"/>
</p>

To create the resource group, log into your Azure portal and click "Resource group" labeled box (1) in the image above. You can also click the search bar to search for "resource group".

<p align="center">
<img src="https://i.imgur.com/9I7vc4k.png" height="80%" width="80%" alt="img"/>
</p>

Click the "Create" tab at the top left.

<p align="center">
<img src="https://i.imgur.com/3JSk340.png" height="80%" width="80%" alt="img"/>
</p>

In the "Resource group" page, select your Microsoft Azure subscription as shown in box (1). Name your resource group "RG-LAB-02" as shown in box (2). For the region, select "(US) West US 3" as shown in box (3). Now, click the "Review + create" tab in the lower left labeled box (4).

<p align="center">
<img src="https://i.imgur.com/myONUt3.png" height="80%" width="80%" alt="img"/>
</p>

You will see a "Vallidation passed" message, go ahead and click the "Create" tab at the lower left labeled box (2) to create the resource group.

<p align="center">
<img src="https://i.imgur.com/LMmdhWT.png" height="80%" width="80%" alt="img"/>
</p>

The "Resoucre group created" notification indicates that our Resource group was created successfully. You will also see "RG-LAB-02" listed as available Resource group, as shown in the box labeled (2)

<p align="center">
<img src="https://i.imgur.com/Sp3379b.png" height="80%" width="80%" alt="img"/>
</p>

To create virtual machines, click the search bar and search for "virtual machines". Select "Virtual machines" labeled box (2)

<p align="center">
<img src="https://i.imgur.com/H0r1gC7.png" height="80%" width="80%" alt="img"/>
</p>

Click "Create" tab, and then click "Azure virtual machines".

<p align="center">
<img src="https://i.imgur.com/npHLplQ.png" height="80%" width="80%" alt="img"/>
</p>

Select your Azure subscription, select the resource group "RG-LAB-02" we created, and name your virtual machine "VM1". For the region, select "(US) West US 3" and select "No infrastructure redundancy required" as the Availability option. For the Image, select "Windows 10 Pro, version 22H2 - x64 Gen2 (free services eligible)". For the Size, select "Standard _E2s_v3 - 2vcpus, 16 GiB memory". We will use "labuser" as the VM1 Username. check the Licensing box, and click the "Networking" tab at the top

<p align="center">
<img src="https://i.imgur.com/d5USAEa.png" height="80%" width="80%" alt="img"/>
</p>

In the networking section, the virtual network, subnet, and public IP will be automatically created for you.
So make sure they all say "(new)". Then click "Review + create" tab at the lower left.

<p align="center">
<img src="https://i.imgur.com/XnRzI0G.png" height="80%" width="80%" alt="img"/>
</p>

"Deployment is in progess" means that the virtual machine is being created.

<p align="center">
<img src="https://i.imgur.com/da8cy1u.png" height="80%" width="80%" alt="img"/>
</p>

"Your deployment is complete" means that the virtual machine has been created.

<p align="center">
<img src="https://i.imgur.com/d3AJDTY.png" height="80%" width="80%" alt="img"/>
</p>

To create the Linux virtual machine, click the search bar and click "Virtual machines".

<p align="center">
<img src="https://i.imgur.com/hPvLnZT.png" height="80%" width="80%" alt="img"/>
</p>

Click "Create", and then click "Azure virtual machine".

<p align="center">
<img src="https://i.imgur.com/WI2nqUw.png" height="80%" width="80%" alt="img"/>
</p>

Select your Azure subscription and select "RG-LAB-02" for the Resource group

NOTE: We want to make sure that both virtual machines are in the same Resource group.

Name your virtual machine "VM2", for the virtual machine Region, select "(US) West US 3". For Availability options, select "No infrastructure redundancy required". For Image, select "Ubuntu server 20.04 LTS x64 Gen2 (free services eligible)". For Size, select "Standard_E2s_v3 - 2vcpus, 16 GiB memory". For the Authentication type, select "Password", and use "labuser" as your Username. Choose a unique password you can remember, and click the "Networking" tab at the top

<p align="center">
<img src="https://i.imgur.com/8ZdonPu.png" height="80%" width="80%" alt="img"/>
</p>

Make sure your VM2 is on the same virtual network as VM1, which is "VM1-vnet". The Subnet and Public IP will be generated automatically, then click "Review + create" tab at the lower left.

<p align="center">
<img src="https://i.imgur.com/epNjJhH.png" height="80%" width="80%" alt="img"/>
</p>

You will see a "Validation passed" message. Click the "Create" tab at the lower left.

<p align="center">
<img src="https://i.imgur.com/1OeRzOX.png" height="80%" width="80%" alt="img"/>
</p>

"Your deployment is complete" message means that VM2 has been created. Click the search bar and search for "virtual machines". 

<p align="center">
<img src="https://i.imgur.com/lKtFVwZ.pngg" height="80%" width="80%" alt="img"/>
</p>

Click "Virtual machines".

We will go ahead and connect both virtual machines using RDP (Remote Destop Protocol).

<p align="center">
<img src="https://i.imgur.com/HGxTcyH.png" height="80%" width="80%" alt="img"/>
</p>

Click "VM1".

<p align="center">
<img src="https://i.imgur.com/Jph5PHG.png" height="80%" width="80%" alt="img"/>
</p>

Copy the Public IP of VM1

<p align="center">
<img src="https://i.imgur.com/wdB60b4.png" height="80%" width="80%" alt="img"/>
</p>

On your local computer, click the search bar, search for "remote desktop", and click "open" to open RDP.

<p align="center">
<img src="https://i.imgur.com/4yiLN1j.png" height="80%" width="80%" alt="img"/>
</p>

Paste the VM1 public IP, and click Connect.

<p align="center">
<img src="https://i.imgur.com/Tfdsy2a.png" height="80%" width="80%" alt="img"/>
</p>

Click "More choices" > "Use a different account", type in VM1 username and password, and click the "Ok" button.

<p align="center">
<img src="https://i.imgur.com/MRxCz7K.png" height="80%" width="80%" alt="img"/>
</p>

We are now connected to VM1, you can choose "No" for all the options as shown in the above image. Click the "Accept" button at the lower right to proceed.

<p align="center">
<img src="https://i.imgur.com/HWKEyKK.png" height="80%" width="80%" alt="img"/>
</p>

Click the "Yes" button.

<p align="center">
<img src="https://i.imgur.com/ymZmp3u.png" height="80%" width="80%" alt="img"/>
</p>

On your VM1 desktop, click the "Microsoft Edge" application to open it

<p align="center">
<img src="https://i.imgur.com/gT61gLQ.png" height="80%" width="80%" alt="img"/>
</p>

Select "Start without your data" > "Confirm and continue" > "Continue without this data" > "Confirm and start browsing".

<p align="center">
<img src="https://i.imgur.com/ymLBHYt.png" height="80%" width="80%" alt="img"/>
</p>

In the search bar, search for "wireshark download", and click Enter.

<p align="center">
<img src="https://i.imgur.com/UwtpLhY.png" height="80%" width="80%" alt="img"/>
</p>

Click on the first link on the web page.

<p align="center">
<img src="https://i.imgur.com/Y9QHjes.png" height="80%" width="80%" alt="img"/>
</p>

Click "Windows x64 Installer" and click the three dots (...) at the top right of the page. click the downloaded Wireshark application to proceed with installation.

<p align="center">
<img src="https://i.imgur.com/axgGzQF.png" height="80%" width="80%" alt="img"/>
</p>

Click "Next" > "Noted" > "Next" > "Next" > "Next" > "Next" > "Install" > "I Agree" > "Install" > "Next" > "Finish" > "Next" > "Finish". 

You've now successfully installed Wireshark on your Windows 10 Pro VM.

<p align="center">
<img src="https://i.imgur.com/tanIfmu.png" height="80%" width="80%" alt="img"/>
</p>

In VM1, search for "Wireshark" on the search bar and click "Open".

<p align="center">
<img src="https://i.imgur.com/hUqgGs1.png" height="80%" width="80%" alt="img"/>
</p>

Select "Ethernet" and click the blue wireshark icon at the top left to start capturing packets.

<p align="center">
<img src="https://i.imgur.com/LxsYwHS.png" height="80%" width="80%" alt="img"/>
</p>

You can see the live traffic that is happening on our virtual machine.

Let's go ahead and filter the traffic so that it stops spamming.

<p align="center">
<img src="https://i.imgur.com/jJADguZ.png" height="80%" width="80%" alt="img"/>
</p>

Search for "icmp" on the search bar, select "icmp" from the list of options provided and press "Enter" on your keyboard

NOTE: ICMP (Internet Control Messaging Protocol) is a network layer protocol used by network devices to communicate errors or other information to other devices (test connectivity to different hosts on a network).

In this case, we will use it to test connectivity to VM2 by pinging VM2's private IP address.

<p align="center">
<img src="https://i.imgur.com/uFLN32h.png" height="80%" width="80%" alt="img"/>
</p>

Go back to your Azure portal and click VM2. Take note of VM2's private IP address.

<p align="center">
<img src="https://i.imgur.com/ikAncIa.png" height="80%" width="80%" alt="img"/>
</p>

Go back to VM1 remote desktop connection, search for "powershell" in the search bar, and click open.

<p align="center">
<img src="https://i.imgur.com/83QW6rm.png" height="80%" width="80%" alt="img"/>
</p>

In Powershell, ping VM2's private IP address by typing "ping 10.0.0.5" and pressing the Enter button on your keyboard.

The image above shows that our ping was successful, as indicated by the 4 replies we got from VM2 (10.0.0.5).

The Ping statistics show that 4 packets were sent and received, and 1 packet was lost.

You can also confirm this on the Wireshark app, which shows us the source and destination IP addresses (VM1 and VM2) and the protocol used (ICMP). It also shows us that four requests was sent and we received four replies

<p align="center">
<img src="https://i.imgur.com/mgtx50E.png" height="80%" width="80%" alt="img"/>
</p>

Let's ping www.comptia.org (ping wwww.comptia.org -4). The -4 means that we are specifying ICMP to ping www.comptia.org IPV4 address.

As you can see from the image above, we got 4 replies, 4 packets were sent and received, and 1 packet was lost.

In the Wireshark app, you can see the source and destination IP addresses of VM1 (10.0.0.4) and www.comptia.org (104.18.16.29).

<p align="center">
<img src="https://i.imgur.com/oPHdkBZ.png" height="80%" width="80%" alt="img"/>
</p>

We will now initiate a non-stop ping from VM1 to VM2.

Let's clear the current ICMP traffic by clicking the green symbol and selecting "Continue without Saving."

<p align="center">
<img src="https://i.imgur.com/dWN9x57.png" height="80%" width="80%" alt="img"/>
</p>

In Powershell, initiate a non-stop ping to VM2 by typing "ping 10.0.0.5 -t", where -t means non-stop.

Non-stop pinging is now initiated.

Let's change the firewall setting of VM2 to not allow ICMP traffic to come through.

<p align="center">
<img src="https://i.imgur.com/1bsnJKD.png" height="80%" width="80%" alt="img"/>
</p>

Go back to your Azure portal and search for "network security group" in the search bar. Click "Network security groups".

<p align="center">
<img src="https://i.imgur.com/ofgQYls.png" height="80%" width="80%" alt="img"/>
</p>

From the above image, you can see both VMs have separate network security groups. Click "VM2-nsg" to open VM2's network security group.

<p align="center">
<img src="https://i.imgur.com/IhHNfxH.png" height="80%" width="80%" alt="img"/>
</p>

In the "VM2-nsg" page, click "Inbound security rules".

NOTE: The "Inbound security rules" page allows us to deny inbound ICMP traffic so that it blocks the pings coming from VM1. We will create a new security rule that denies ICMP traffic.

<p align="center">
<img src="https://i.imgur.com/fAuKcKp.png" height="80%" width="80%" alt="img"/>
</p>

Click the "Add" tab at the top of the page, leave "Source", "Source port ranges", "Destination", "Services" as default. For the Protocol, select "ICMP", you will notice the "Destination port ranges" changes to asterisk (*). For Action, select "Deny", and for Priority, type "200".

NOTE: Priority determines the order in which the rule is processed. Rules are processed in order, from lowest priority to highest priority. The lower the number, the higher the priority.

Name the rule "DENY_ICMP_PING_FROM_ANYWHERE", and click the "Add" button. Go back to VM1 and observe the ping request and traffic.

<p align="center">
<img src="https://i.imgur.com/962OsqK.png" height="80%" width="80%" alt="img"/>
</p>

You can see the ping requests have started timing out on PowerShell (they're getting blocked by VM2's firewall).

Observe the traffic on Wireshark. You will notice that there's only request but no response.

Go back to Azure portal and allow ICMP traffic

<p align="center">
<img src="https://i.imgur.com/Nnosiea.png" height="80%" width="80%" alt="img"/>
</p>

Click the new rule we added earlier "DENY_ICMP_PING_FROM_ANYWHERE". 

<p align="center">
<img src="https://i.imgur.com/raDnTHO.png" height="80%" width="80%" alt="img"/>
</p>

Select "Allow" for Action, and click the "Save" button.

<p align="center">
<img src="https://i.imgur.com/RkmbKLo.png" height="80%" width="80%" alt="img"/>
</p>

The "Action" column now says allow. Go back to VM1 and observe Powershell ping requests and Wireshark traffic.

<p align="center">
<img src="https://i.imgur.com/B3Cd9Hj.png" height="80%" width="80%" alt="img"/>
</p>

By observing PowerShell and Wireshark, you will notice we're now getting replies from VM2.

Click PowerShell and press "Ctrl + C" on your keyboard to stop the non-stop pings.

Now, let's observe SSH traffic on Wireshark.

NOTE: SSH (Secure Shell) is a network protocol that provides a secure way to access a remote computer. We will connect to VM2 from VM1 through SSH.

<p align="center">
<img src="https://i.imgur.com/p9jJIC9.png" height="80%" width="80%" alt="img"/>
</p>

In Wireshark, click the green Wireshark symbol at the top and click "Continue without Saving".

<p align="center">
<img src="https://i.imgur.com/O1uDgCp.png" height="80%" width="80%" alt="img"/>
</p>

Type "ssh" in the search bar and press the Enter button on your keyboard to filter for ssh traffic only.

<p align="center">
<img src="https://i.imgur.com/MO9NEn9.png" height="80%" width="80%" alt="img"/>
</p>

In VM1, we will now ssh into VM2 by typing "ssh labuser@10.0.0.5" in PowerShell and press Enter.

NOTE: "labuser" is VM2's username, and "10.0.0.5" is VM2's private IP address.

Notice the ssh traffic on Wireshark.

<p align="center">
<img src="https://i.imgur.com/XXiPRU9.png" height="80%" width="80%" alt="img"/>
</p>

Type "yes" when asked "Are you sure you want to continue connecting".

Type in your VM2 password (this is the password we used when we creating Linux virtual machine).

NOTE: The password won't be visible when you are typing it in PowerShell.

We have now successfully connected to VM2 through SSH.

<p align="center">
<img src="https://i.imgur.com/LwX0CWE.png" height="80%" width="80%" alt="img"/>
</p>

Let's run some Linux commands.

Type "id" and press Enter to print the user ID of VM2.

Type "uname -a" and press Enter to print all of the system information about VM2 (including the kernel name, hostname, kernel version, operating system, machine hardware name, and processor architecture).

Type "pwd" and press Enter to print the current working directory.

Type "ls lasth" to list the contents of the current directory in long format.

Type "touch -hi.txt" and press enter to create a text file.

Type "ls lasth" and press Enter to list the text file.

Type "exit" and press Enter to close SSH connection to VM2.

Notice that whenever we type a command on Powershell, it goes over the network and spams ssh traffic on WireShark.

<p align="center">
<img src="https://i.imgur.com/CswkSOF.png" height="80%" width="80%" alt="img"/>
</p>

We are now back to VM2.

Type "ipconfig" and press Enter. This will display the private IP of VM1, which is 10.0.0.4.

<p align="center">
<img src="https://i.imgur.com/PbexjNq.png" height="80%" width="80%" alt="img"/>
</p>

Let's filter DHCP traffic on WireShark.

NOTE: DHCP (Dynamic Host Configuration Protocol) is a network management protocol that automatically assigns IP addresses and other network configuration parameters to devices on a network.

On WireShark, filter for DHCP traffic by typing "dhcp" in the search bar and pressing Enter.

<p align="center">
<img src="https://i.imgur.com/z6iGsQR.png" height="80%" width="80%" alt="img"/>
</p>

In PowerShell, type "ipconfig /renew". This will reissue our private IP address.

Observe the DHCP traffic in WireShark.

Let's filter DNS traffic on WireShark.

<p align="center">
<img src="https://i.imgur.com/nZHJR06.png" height="80%" width="80%" alt="img"/>
</p>

NOTE: DNS (Domain Name System) translates domain names meaningful to humans into numerical IP addresses.

In WireShark, type dns in the search bar and press Enter. You will notice the DNS traffic spam on WireShark.

<p align="center">
<img src="https://i.imgur.com/TzFwjff.png" height="80%" width="80%" alt="img"/>
</p>

Click the green WireShark icon at the top, and click "Continue without Saving". This will clear current DNS traffic.

<p align="center">
<img src="https://i.imgur.com/r9nft4s.png" height="80%" width="80%" alt="img"/>
</p>

In PowerShell, type "nslookup www.comptia.org", this will query Internet domain name servers (DNS) to obtain information about www.comptia.org domain names or other DNS records.

Observe the DNS traffics in WireShark (the source and destination IP addresses). 

<p align="center">
<img src="https://i.imgur.com/ICklCv8.png" height="80%" width="80%" alt="img"/>
</p>

Let's lookup www.google.com IP address.

In WireShark, instead of typing "dns" in the search bar, we can also filter DNS traffics by typing "udp.port == 53" in the search bar (since DNS uses port 53). Press Enter button on your keyboard.

Click the green WireShark icon at the top, and click "Continue without Saving". 

<p align="center">
<img src="https://i.imgur.com/8DDokqd.png" height="80%" width="80%" alt="img"/>
</p>

In PowerShell, type "nslookup wwww.google.com" and press Enter. Observe Google's IP addresses, and in WireShark, observe our source and destination IP addresses. Also, observe the protocol used (DNS).

Let's filter for RDP traffic.

<p align="center">
<img src="https://i.imgur.com/u51v2mK.png" height="80%" width="80%" alt="img"/>
</p>

NOTE: RDP (Remote Desktop Protocol) provides users with a graphical interface to connect (remote access) to another computer over a network connection.

Since RDP uses port 3389, we will type "tcp.port == 3389" in the Wireshark search bar and press Enter. You will notice a non-stop traffic spamming on WireShark. The reason is because the RDP (protocol) is constantly showing us a live stream from our local computer to our virtual machine, therefore traffic is always being transmitted.

<p align="center">
<img src="https://i.imgur.com/MZUK2eB.png" height="80%" width="80%" alt="img"/>
</p>

Let's exit from our Virtual machine by clicking the "X" button and clicking "Ok".

<p align="center">
<img src="https://i.imgur.com/0RbpaRJ.png" height="80%" width="80%" alt="img"/>
</p>

Let's delete our resource group so we won't get charged.

In your Azure portal, click the search bar and select "Resource groups". 

<p align="center">
<img src="https://i.imgur.com/0trdXVN.png" height="80%" width="80%" alt="img"/>
</p>

Click "RB-LAB-02"

<p align="center">
<img src="https://i.imgur.com/jg41HDd.png" height="80%" width="80%" alt="img"/>
</p>

Click "Delete resource group" at the top, type in the name of your resource group "RG-LAB-02", click the "Delete" button, and click "Delete".

<p align="center">
<img src="https://i.imgur.com/Ggzs0ol.png" height="80%" width="80%" alt="img"/>
</p>

Let's confirm the deletion by clicking the notification tab at the top, you can see our resource group has been deleted.




















