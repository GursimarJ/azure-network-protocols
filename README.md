<p align="center">
<img src="https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/WIRESHARKimg.png" height="40%" width="70%"alt="Wireshark Logo"/>


<h1>Network Security Groups (NSGs) and Inspecting Network Protocols</h1>
In this walkthrough, we will set up two virtual machines (VMs) in Microsoft Azure. Our objective is to monitor and analyze the network traffic between the two VMs using Wireshark. In addition to capturing direct communication, we will also observe background traffic and other types of network activity that occur within the virtual environment.<br />

<h2>Languages</h2>

- PowerShell 

<h2>Environments Used</h2>

- Microsoft Azure 
- Windows 10
- Linux Ubuntu
- Windows App(Remote Desktop)
- PowerShell


<h2>Technology/Applications/Services </h2>

- Azure Virtual Machines
- Wireshark
- PowerShell


<h2>Walkthrough</h2>

</br>
<h3 align="center">
  Setting up the Virtual Machines
</h3>
</br>

First, a resource group needs to be created so our VM’s can be put in it.
  
![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic1.png)
</p>
<br />
<p>
Create a Windows 10 Virtual Machine (VM)</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Same resource group, new Vnet, & new Subnet

  


![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic2.png)
</p>
<br />
<p>
Create a Linux (Ubuntu) VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Same resource group, Vnet & Subnet as the Windows VM

</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic3.png)
</p>
</br>
<h3 align="center">
  Observing ICMP Traffic
</h3>
</br>

Use Windows App(Remote Desktop), to log in to the Windows VM.



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic4.png)
</p>
<br />
<p>
Install Wireshark in the Windows VM
</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic5.png)
</p>
<br />
<p>
Open Wireshark and start packet capture, by selecting an interface and then clicking start capturing packets button.
</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic6.png)
</p>
<br />
<p>
Within Wireshark, filter for ICMP traffic only

![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic7.png)
 
</p>
</br>
Get the Linux VM’s private IP address(10.0.0.5) and from within the Windows VM, use PowerShell to ping the Linux VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type “ping 10.0.0.5”</br>
*As a result, ICMP traffic is displayed in purple on Wireshark





![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic8.png)
</p>
</br>
<h3 align="center">
  Configuring a Firewall [Network Security Group]
</h3>
</br>

Initiate a perpetual/non-stop ping from the Windows VM to the Ubuntu VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - type “ping 10.0.0.5 -t”




![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic9.png)
</p>
<br />


Open the Network Security Group the Ubuntu VM is using and disable incoming (inbound) ICMP traffic</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Azure Portal -> Linux VM(that we created) -> Networking -> Network Settings -> Click nsg link under Network Security Group -> Settings -> Inbound Security Rules -> Add and set up based on the following picture(Deny ICMPv4)

![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic10.png)
</p>
<br />
<p>
Back in the Windows VM, observe the ICMP traffic in WireShark and the command line Ping activity</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Once our rule is active, we will stop getting a reply from the Linux VM(10.0.0.5).

</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic11.png)
</p>
<br />
<p>
Let's say we simply delete the rule that we made: as a result, we will start getting replies from the Linux VM again.

</p>




![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic12.png)
</p>
</br>
<h3 align="center">
  Observe SSH Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for SSH traffic only



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic13.png)
</p>
<br />
<p>
From the Windows VM we will establish an SSH connection with the Linux VM</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell and type: ssh <username>@<private IP address>. In our case, private IP address for Linux VM is 10.0.0.5


</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic14.png)
</p>
<br />
<p>
Type commands (username, pwd, etc) into the linux SSH connection and observe SSH(tcp.port 22) traffic spam in WireShark
</p>

![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic15.png)
</p>
<br />
<p>
We can exit the SSH connection by typing ‘exit’ and pressing [Enter] in PowerShell, and on Wireshark we can see a RST, ACK packet that indicates the connection was ended. 

</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic16.png)
</p>
</br>
<h3 align="center">
  Observe DHCP Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for DHCP traffic only



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic17.png)
</p>
</br>
We will attempt to issue our Windows VM a new IP address and see the DHCP traffic in the background.</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell as admin and run: ipconfig /renew


![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic18.png)
</p>
<br />

*
Since we are on a Virtual Machine, above isn’t normal behavior when running ipconfig /renew. Usually, the renew command is run along with ipconfig /release(first release then renew). If we run the release command on its own, then we will disconnect from our VM.

Something we can do to avoid this problem is to run a script so the release and renew command can be run, such that the renew occurs right after the release with no time in between.



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic19.png)
</p>
<br />
<p>
Then the private IP address will be renewed properly and Discover, Offer, Request, and Acknowledge packets can be seen.
</p>



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic20.png)
</p>
</br>
<h3 align="center">
  Observe DNS Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for DNS traffic only





![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic21.png)
</p>
</br>
We will make DNS traffic(tcp.port 53) by going to PowerShell and looking up the IP address of a website.</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Open PowerShell and type: nslookup disney.com



![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic22.png)

<br />

Here is the traffic generated on Wireshark as a result.





![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic23.png)
</p>
</br>
<h3 align="center">
  Observe RDP Traffic
</h3>
</br>

On Windows VM, start packet capture on Wireshark and filter for RDP traffic only</br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; - Right after entering tcp.port==3389 filter there is constant traffic. This is because we are on a Virtual Machine and RDP traffic is constantly occurring to stream the VM on our host machine.





![image alt](https://github.com/GursimarJ/azure-network-protocols/blob/26ae840f266008be3184bf725011686faaf86e37/assets/Part5Pics/Part5Pic24.png)
