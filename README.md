<h1>Exploring Network Security Groups (NSGs) and Monitoring Azure VM Traffic Using Wireshark</h1>
<p>This guide walks you through inspecting network interactions between Azure Virtual Machines using Wireshark and experimenting with NSG rules to understand their impact on traffic flow.</p>

<h2>Tools and Platforms Utilized</h2>
<ul>
  <li>Microsoft Azure (Compute Resources/Virtual Machines)</li>
  <li>Remote Desktop Protocol (RDP)</li>
  <li>Terminal Commands</li>
  <li>Common Network Protocols (ICMP, SSH, DHCP, DNS, RDP)</li>
  <li>Wireshark (Network Traffic Analyzer)</li>
</ul>

<h2>Operating Systems Applied</h2>
<ul>
  <li>Windows 10 (21H2 version)</li>
  <li>Ubuntu Server 20.04 LTS</li>
</ul>

<h2>Summary of Key Tasks</h2>
<ul>
  <li>Build a Resource Group</li>
  <li>Deploy Virtual Machines</li>
  <li>Monitor ICMP Data</li>
  <li>Inspect SSH Connections</li>
  <li>Analyze DHCP Requests</li>
  <li>Capture DNS Lookups</li>
  <li>View RDP Sessions</li>
</ul>

<h2>Step-by-Step Walkthrough</h2>
<br><br>
<h3 align="center">Begin Setting Up Your Lab</h3>
<br>
<p>Start by creating a Resource Group in your Azure subscription to keep your environment organized.</p>
<p><img src="https://i.imgur.com/dOAeXqs.png" width="100%" alt="Resource Group"></p>

<p>Next, set up a Windows Virtual Machine. A common region for deployment is East US.</p>
<p>Assign it to the previously created Resource Group and let Azure automatically provision a new Virtual Network and Subnet. Be sure to select the password option under <strong>Administrator Account</strong>.</p>
<p><img src="https://i.imgur.com/PHOwjLh.png" width="100%" alt="Windows VM"></p>

<p>Now, create an Ubuntu Virtual Machine in the same Resource Group. Again, allow Azure to generate the associated VNet and Subnet. Choose the password login option.</p>
<p><img src="https://i.imgur.com/N5zwQUH.png" width="100%" alt="Ubuntu VM"></p>

<p>Use Network Watcher to verify your network's structure and connectivity:</p>
<p><img src="https://i.imgur.com/Pn02GXF.png" width="100%" alt="Network Watcher"></p>

<br><br>
<h3 align="center">Capturing ICMP (Ping) Activity</h3>
<br>
<p>Access the Windows VM remotely, install Wireshark, and filter the display to show only ICMP traffic.</p>
<p><img src="https://i.imgur.com/0BsfNiS.jpg" width="100%" alt="Microsoft Remote Desktop - Mac"></p>

<p>Locate your Ubuntu VM’s internal IP and send ping requests from the Windows VM. Check Wireshark for request/reply patterns:</p>
<p><img src="https://i.imgur.com/yYGKuAy.png" width="100%" alt="Ubuntu private IP">
<img src="https://i.imgur.com/3h9QSEY.png" width="100%" alt="ICMP traffic - private IP"></p>

<p>Try pinging a public domain like google.com and watch for related traffic in Wireshark:</p>
<p><img src="https://i.imgur.com/YduMvc7.png" width="100%" alt="ICMP traffic - public IP"></p>

<p>Set up continuous pinging to the Ubuntu machine and observe the flow:</p>
<p><img src="https://i.imgur.com/bihftKK.png" width="100%" alt="ICMP traffic - perpetual ping"></p>

<p>Modify the NSG settings on the Ubuntu VM to block ICMP and view the dropped traffic results in Wireshark and the command prompt:</p>
<p><img src="https://i.imgur.com/ovGk5dq.png" width="100%" alt="ICMP traffic - blocked">
<img src="https://i.imgur.com/NjuUANI.png" width="100%" alt="ICMP traffic - denied"></p>

<p>Re-enable ICMP in the NSG and check if connectivity resumes. End the continuous ping afterward:</p>
<p><img src="https://i.imgur.com/nZbl2sA.png" width="100%" alt="ICMP traffic - allowed again"></p>

<br><br>
<h3 align="center">Monitoring SSH Traffic</h3>
<br>
<p>Filter Wireshark for SSH data. From your Windows VM, establish an SSH session to the Ubuntu server using its internal IP. Run some commands (like <code>ls</code> or <code>pwd</code>) to generate traffic.</p>
<p>End the session by typing <code>exit</code> and pressing Enter.</p>
<p><img src="https://i.imgur.com/6YEDJKu.png" width="100%" alt="SSH traffic"></p>

<br><br>
<h3 align="center">Watching DHCP Communications</h3>
<br>
<p>Switch Wireshark to view only DHCP traffic. On the Windows VM, run <code>ipconfig /renew</code> to request a new IP.</p>
<p>Observe DHCP discovery, offer, request, and acknowledgment in Wireshark:</p>
<p><img src="https://i.imgur.com/mKyAHFr.png" width="100%" alt="DHCP traffic"></p>

<br><br>
<h3 align="center">Viewing DNS Query Activity</h3>
<br>
<p>Apply a DNS filter in Wireshark. Use the <code>nslookup</code> command from your Windows VM to query domains such as google.com and disney.com.</p>
<p>Check Wireshark for name resolution packets:</p>
<p><img src="https://i.imgur.com/mYZ8CAK.png" width="100%" alt="DNS traffic"></p>

<br><br>
<h3 align="center">Inspecting RDP Data Streams</h3>
<br>
<p>Filter Wireshark with <code>tcp.port==3389</code> to observe RDP session traffic.</p>
<p>Notice a continuous stream of data—this is due to RDP constantly transmitting screen changes and input across systems.</p>
<p><img src="https://i.imgur.com/hNlhTVp.png" width="100%" alt="RDP traffic"></p>

<p>Once finished, be sure to clean up! Remove all created Azure resources to avoid unexpected charges.</p>
<p>Close out of your remote connections, delete the Resource Groups, and confirm their removal in the Azure portal notifications.</p>
