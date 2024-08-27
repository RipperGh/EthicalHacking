## Task: Configure a static IP address v4

# Step 1
  - launch powershel
# Step 2: Static IP
  - Type in : netsh interface ip set address name="Ethernet 2" static 192.168.0.50 255.255.255.0

# Step 3: Preferred DNS Server
 - type: netsh interface ip set dns "Ethernet 2" static 192.168.0.2

# Step 4
  - ipconfig /all

  ```
  IPv4 Address: 192.168.0.50
  Subnet Mask: 255.255.255.0
  DNS Servers: 192.168.0.2
  ```
--------
## Second task: Enable IP Routing 

# Step 1:
  - while in the PowerShell prompt, type regedit and press Enter.
    -  Will open Registry Editor
# Step 2: 
- From the console tree on the left pane, expand the HKEY_LOCAL_MACHINE node, if not already expanded.
# Step 3: 
  - Under the HKEY_LOCAL_MACHINE node, expand to the SYSTEM-> CurrentControlSet > Services > Tcpip node.]

# Step 4:
  - Click Parameters under the Tcpip node. -> 1On the right pane, right-click IPEnableRouter and select Modify.
# Step 5: 
  - On the Edit DWORD (32-bit) Value dialog box, click in the Value data text box and type: 1

------- 
## Task 3: Disable IPv6 on the Domain Controller

# Step 1:
- Right-click the network icon on the system tray and select Open Network and Sharing Centre.

# Step 2:
- On Network and Sharing Center, click Change adapter settings from the left pane.

# Step 3:
  - On the Network Connections window, right-click Ethernet 2 and select Properties.

# Step 4 : 
- On the Ethernet 2 Properties dialog box, clear the Internet Protocol Version 6 (TCP/IPv6) check box. -> Click OK.

---------
## Task 4: Configure an IPv6 Router Advertisement for Global Address 2001: 

# Step 1: 
- On the open PowerShell window, type the following to clear all previous commands: clear
- netsh interface ipv6 set interface "Ethernet 2" forwarding=enabled advertise=enabled

# Step 2:
  - Notice that the system responds to the command with an Ok. This confirms that the advertising is now enabled. On the next PowerShell prompt, type the following command: netsh interface ipv6 add route 2001:db8:0:1::/64 "Ethernet 2" publish=yes


----
## Task 5: Verify IPv6 Configuration

# Step 1
  - ipconfig to show if enabled 
------

## Task 6 :  Add the PLABSA01 to the PRACTICELABS.COM domain

# Step 1 
  - Windows PowerShell window and type the following commands one-by-one. Press Enter after every command.
    ```
    netsh interface ip set dns "Ethernet" static 192.168.0.2
    netsh interface ip set dns "Ethernet 2" static 192.168.0.2
    ```

# Step 2
 - PowerShell prompt, type:
```
Add-Computer -DomainCredential practicelabs\administrator -DomainName practicelabs.com
```
will reboot 

-------
## Task 7: Add Host Records of PLABSA01 in DNS Server

# Step 1 
  - Server Manager window then click Tools from the menu on the top, and select DNS.
# Step 2 
  - In the DNS Manager, expand the name of DNS node under the console tree on the left pane.Under the node, expand the Forward Lookup Zones node and then expand PRACTICELABS.COM

# Step 3 
  - Right-click PRACTICELABS.COM and select New Host (A or AAAA).

# Step 4 
  - In the New Host dialog box, specify the following values:
    ```
    Name (uses parent domain name if blank): ISATAP
    IP address: 192.168.0.3
    ```
# Step 5 
  - The DNS dialog box appears informing that the record was setup successfully.Click OK to close the dialog box.
# Step 6 
  - Again, on the New Host dialog box, specify the following values:
    ```
    Name (uses parent domain name if blank): ISATAP
    IP address: 192.168.0.3
    ```
------

## Task 8: Configure ISATAP router on PLABSA01

# Step 1
  - Open PowerShell
# Step 2
  - On the PowerShell prompt, type:
  ```
  netsh interface ipv6 isatap set router 192.168.0.3
  ```
# Step 3 
  - On the next PowerShell prompt, type: ipconfig to view - >  {6B62B13F-42F4-419A-A3FC-OD99E98CBAAE}

# Step 4 
  - netsh interface ipv6 set interface "isatap.{6B62B13F-42F4-419A-A3FC-OD99E98CBAAE}" forwarding=enabled advertise=enabled

# Step 5
  - netsh interface ipv6 add route 2001:db8:0:10::/64 "isatap.{6B62B13F-42F4-419A-A3FC-OD99E98CBAAE}" publish=yes
----
## Task 8: Enable ISATAP on an IPv4-only Domain Server

Step 1 
  - Launch Powershell enter: netsh interface isatap set router 192.168.0.3
----
## Task 9: Set Firewall Rules to Enable Network Connectivity

# Step 1
-  right-click the Network icon at the bottom right and click Open Network and Sharing Center.

# Step 2
  - Click on the Windows Firewall link at the bottom right.
# Step 3
 - Click the Use recommended settings button to enable the Windows Firewall on the device.

# Step 4
  - The firewall indicators turn green. Click on the Advanced Settings link on the left.

# Step 5 
  - On the Windows Firewall with Advanced Security window, click Inbound Rules in the left pane of the console tree. List of Inbound Rules is displayed in the middle pane.
# Step 6 
  - Scroll down and find the File and Printer Sharing (Echo Request - ICMPv4-In) rule and verify that it has a green tick appended to it.
# Step 7 
  - Find the File and Printer Sharing (Echo Request - ICMPv6-In) rule just below it and verify that it too has a green tick appended to it.
-----
## Task 10:Verify Connectivity between IPv4-only and IPv6-only Devices on the Network
 # Step 1 
   - Windows PowerShell window and type:ping plabdc01

# Step 2
  - response is from the global IPv6 address provided by the ISATAP router. On the next PowerShell prompt, type: ping 192.168.0.2
