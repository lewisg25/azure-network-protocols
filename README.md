# azure-network-protocols
<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
  </p>
  <h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>

  <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
  <p>
    Welcome to this tutorial on configuring Network Security Groups (NSGs) and inspecting network protocols within your Azure environment.</p>

  <p><b>Environments and Technologies used in this Demonstration:</b> <br>
    <ul> 
      <li>Microsoft Azure </li>
      <li> Remote Desktop Protocol</li>
      <li> Various Command-Line Tools</li>
      <li>Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP) </li>
      <li>Wireshark (Protocol Analyzer) </li>
    </ul>
        </p>
        <p><b>Operating Systems:</b> <br>
        <ul> 
      <li> Windows 10</li>
      <li> Ubuntu Server 20.04</li>
      </ul>
          </p> 
      <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
      <p>To begin, create two virtual machines (VMs) in Azure: one Linux-based and one running Windows 10. Both VMs should be assigned two CPUs and placed within the same virtual network (VNet) to ensure proper communication.
      </p>
      <p>
      Next, on the Windows 10 VM, download and install Wireshark from the following link: Wireshark Download. Once installation is complete, launch Wireshark and configure it to filter for ICMP traffic only. ICMP (Internet Control Message Protocol) operates at the network layer and is essential for relaying diagnostic information, such as network connectivity issues. The ping command utilizes ICMP to test connectivity between devices.
    </p>
    <p>
      By applying the ICMP filter in Wireshark and executing a ping command to the private IP address of the Linux VM, you will be able to observe the ICMP packets captured in real-time, providing valuable insight into the network traffic between your VMs.    
      </p>
  
  


  <br />
  <p>
  <img src="https://i.imgur.com/IIUShxp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
    We can analyze each individual packet to review the specific data transmitted with each ping. The image below illustrates this process in detail.
  
  </p>
  <br />
  <p>
  <img src="https://i.imgur.com/GLxSIG3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
    In the next phase of the lab, we will initiate a continuous ping to the Linux machine using the ping -t command. This command will maintain an ongoing ping request until manually stopped. While the Windows machine is pinging the Linux machine, we will proceed to the Linux VM and configure the firewall to block inbound ICMP traffic.</p>
<p>
    Upon implementing this firewall rule, the Windows machine will stop receiving echo replies from the Linux machine. To block ICMP traffic, we will create a new Network Security Group (NSG) on the Linux VM and configure it to deny ICMP traffic. Alternatively, ICMP traffic can be allowed again by adjusting the NSG settings on the Linux VM through the Azure portal.  
  </p>
  
  <p>
  <img src="https://i.imgur.com/5vXO75R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
  <img src="https://i.imgur.com/Asl80tN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <h1 style="text-align: center; font-weight: bold; font-size: 16pt;">SSH and Wireshark</h1> </p>
  <hr style="border: 1px solid #ccc; width: 80%; margin: 20px auto;">
  <p>
    Next, we will utilize the Windows machine to establish an SSH connection to the Linux machine. SSH (Secure Shell) operates solely via the command-line interface (CLI) and does not utilize a graphical user interface (GUI), providing secure remote access to the machine.
</p>
<p><b> What is SSH (Secure Shell)</b> <br>
  SSH (Secure Shell- Port 22) is a cryptographic network protocol that provides secure remote access to a computer's command-line interface over an unsecured network. It ensures data confidentiality and integrity by encrypting the communication between the client and server, making it a widely used method for managing remote systems.
</p>
<p><b> What is Wireshark</b> <br>
  Wireshark is a powerful, open-source network protocol analyzer that captures and inspects data packets traveling through a network. It allows users to view detailed information about network traffic, helping with troubleshooting, security analysis, and protocol development.
</p>
<p>
    We will configure Wireshark to filter and capture only SSH packets. Upon executing the SSH connection with the command ssh labuser@10.0.0.5, Wireshark will immediately begin capturing the SSH traffic, allowing us to monitor the encrypted data exchange between the two machines.  
  </p>
  <p>
  <img src="https://i.imgur.com/zteR41r.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
    Next, we will configure Wireshark to filter for DHCP traffic. The Dynamic Host Configuration Protocol (DHCP) operates on ports 67 and 68 and is responsible for dynamically assigning IP addresses to devices on a network.
</p>
<p>
    To observe this process, we will request a new IP address by executing the command ipconfig /renew on the Windows machine. Upon issuing the command, Wireshark will capture and display the DHCP traffic, allowing us to analyze the communication between the client and the DHCP server during the IP address allocation process.  
  </p>
  <p>
  <img src="https://i.imgur.com/vU8fpQf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
    Now, we will configure Wireshark to filter for DNS traffic. DNS (Domain Name System) is responsible for resolving domain names into IP addresses.
</p>
<p>
    To generate DNS traffic, we will initiate a lookup by entering the command nslookup www.google.com. This command queries the DNS server to resolve the domain name "www.google.com" into its corresponding IP address. Wireshark will capture the DNS packets exchanged during this resolution process, allowing us to analyze the details of the request and response.  
  </p>
  <p>
  <img src="https://i.imgur.com/VMcwmsO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
  <p>
    Finally, we will configure Wireshark to filter for RDP (Remote Desktop Protocol) traffic. By applying the filter tcp port 3389, we will capture traffic on the default RDP port.
</p>
<p>
    Once this filter is applied, you will notice continuous traffic being generated as the Remote Desktop Protocol is actively used to establish and maintain a connection to the Virtual Machine. This behavior occurs because RDP operates on port 3389 and continuously exchanges data during an active remote desktop session.  
  </p>
  <p>
  <img src="https://i.imgur.com/VxXGv6X.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  </p>
