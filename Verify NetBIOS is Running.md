# How to check if netbios is running 

  - Enter in CMD: netstat -an

  Details of various active ports on the system are listed.

  Notice the entry - 192.168.0.2:139 - LISTENING. This confirms that port 139 is enabled on your machine.

# How to Block NetBIOS
  - Go to Control Panel
    -   select Network and Internet.
        -   Network and Sharing Center.
            -  select Local Area Connection - > Properties - > Internet Protocol Verison 4  (TCP/IPv4) - > Select Properties
            -  Advance Tab, Then Select WINS, at the bottom there will be a disable NetBIOS over TCP/IP

