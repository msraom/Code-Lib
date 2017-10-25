# Basic Client/Server Security & Data Security Concepts (Debian)
=================
1. Socket: `net.node.port`
2. Packet: Five layers 
   ```
   layer 1 (physical layer)
   layer 2 (link layer) (Ethernet/Frame Header): 14 B
   layer 3 (network layer)   (IP Header): 20 B
   layer 4 (transport layer)  (TCP/UDP/ICMP Header) xx/8/8 B
   layer 5 (data/payload layer)
   ```
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/packet_layers.PNG)
3. IP 5 classes
   ```
   class A: 1-127 (N.H.H.H) (127.0.0.0/8 for loopback)
   class B: 128-191 (N.N.H.H)
   class C: 192-223 (N.N.N.H)
   class D: 224-239 (Reserved for Multicasting)
   class E: 240-254 (Experimental/used for research)
   ```
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/IP_classes.PNG)
4. Private IP addresses
   ```
   class A: 10.0.0.0/8
   class B: 172.16.0.0/16 -> 172.31.0.0/16
   class C: 192.168.0.0/16
   ```
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/private_IP.PNG)
5. Headers structure  
   IP header:  
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/IP_header.PNG)
   TCP header:  
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/TCP_header.PNG)
   UDP header:  
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/UDP_header.PNG)
   ICMP header:  
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/ICMP_header.PNG)
6. Linux structure
   Kernel -> Module -> Daemon -> Applications  
   ![alt text](https://github.com/mndarren/Code-Lib/blob/master/Data_Security_lib/resource/pic/Linux_structure.PNG)
7. /usr/sbin: Unix System Resources/System binary
8. TCP flags
   ```
   U -URG --Urgent       ---Unskilled
   A -ACK --Ackownlege   ---Attacker
   P -PSH --Push         ---Pester
   R -RST --Reset        ---Real
   S -SYN --Synchronize  ---Security
   F -FIN --Final        ---Folks
   ```
9. Important config files
   ```
   /etc/fstab     # accessible fs
   /etc/motd      # message of today
   /etc/hosts     # names to ip addresses
   /etc/networks  # network names to ip addresses
   /etc/protocols # contain all protocols
   /etc/services  # contain all services
   /etc/pid#/maps # view process in memory (rwsp, read|write|shared|protected)
   ```
10. Port types
   ```
   encrypted port (Ex: 22)
   redirected port (Ex: ncat -v -c 'echo this is a test' -l -p 1234 -t)
   status port (Ex: rpcinfo -p 10.18.59.34)
   rogue port (Ex: you don't know)
   ```
11. Digital Signature (used to confirm Data Integration)
   ```
   Type    |  Hex  |  bits
   sum        5       20
   cksum      10      40
   md5sum     32      128
   sha128sum  32      128
   sha512sum  124     512
   ```
12. Top 20 ports
   ```
   Echo        7         TCP/UDP
   FTP         20/21     TCP
   SFTP        115       TCP      --encrypted
   FTPS        989/990   TCP      --encrypted
   SSH/SCP     22        TCP      --encrypted
   Telnet      23        TCP
   SMTP        25        TCP      --used for sending email
   SMTPS       465       TCP      --encrypted
   POP3        110       TCP      --used for receiving email
   POP3S       995       TCP      --encrypted
   DNS         53        TCP
   DHCP        67/68     UDP
   HTTP        80        TCP
   HTTPS       443       TCP      --encrypted
   Kerberos    88        TCP/UDP  --464 for Kerberos v5
   sunrpc      111   
   SNMP        161/162
   LDAP        389   
   LDAPS       636       TCP/UDP  --encrypted
   NFS         2049
   Protected   0-1023
   ```   
13. Terms
   ```
   MAC  --Media Access Control
   LLC  --Logical Link Control
   DNS  --Domain Name System
   TCP  --Transission Control Protocol (6)
   UDP  --User Datagram Protocol (17)
   ICMP --Internet Control Message Protocol (1)
   ARP  --Address Resolution Protocol
   LDAP --Lightweight Directory Access Protocol
   DHCP --Dynamic Host Configuration Protocol
   HTTP --Hypertext Transfer Protocol
   SNMP --Simple Network Management Protocol
   CSMA/CD --Carrier Sensor Multiple Access/Collision Detection
   UUCP --Unix to Unix Copy Protocol
   UUID --Universally Unique Identifiers
   SSL  --Security Socket Layer
   NAS  --Network Attached Storage
   RAID --Redundant Array of Independant Disks
   MBR  --Master Boot Record
   VBR  --Volume Boot Record
   Nyble--4 bits, half byte, nibble, nybble
   PTR  --Public Test Region, used to reverse IP addr
   VPN  --Virtual Private Network (double encrypted)
   PID  --a chunk of code in memory
   OSPF --Open Shortest Path First
   ```
14. 7 file types
   ```
   -  --file
   d  --directory
   p  --pipe
   c  --character
   l  --link
   b  --block
   s  --socket
   ```
15. TTL default length
   ```
   64   --Linux default
   128  --Windows default
   255  --Cisco default
   ```
16. Diff between Reject connection and Drop connection   
   ```
   1) Deny(Reject,sending an ICMP destination-unreachable back): close server -> client tried to connect server -> kernel reply refused for safety
   2) Drop: (prohibit a packet from passing. send no response)
   Create a new chain:
   iptables -N LOG_AND_DROP
   iptables -A LOG_AND_DROP -j LOG --log-prefix "Source host denied "
   iptables -A LOG_AND_DROP -j DROP
   Use this chain:
   iptables -A INPUT -s z.z.z.z/32 -j LOG_AND_DROP
   iptables -A INPUT -s y.y.y.y/32 -j LOG_AND_DROP
   iptables -A INPUT -s a.a.a.a/32 -j LOG_AND_DROP
   Note: Make sure you flush existing rules before adding the above rules. Otherwise you'll still have the misleading rule in there that is logging but not denying.
   ```
17. **English Domain** --(DNS)--> **Number IP address** --(ARP)--> **Data Link(MAC)**
18. Stateful Inspection (See details for Ramdisk in Big_Tools.md)<br/>
   ```
   It's not so respectful because:  
   1) 75% attacks coming at Application level
   2) encryption will not detect the attacks
   ```
19. AS (Autonomous System)  --cloud zones #**Virtual zone environment**
20. Timing Vectors<br/>
    Why do we need it? Because:
    Attacker monitor our port traffic, and use it to spoof us to connect their bogus host. Then they can get our username and password. Timing Vectors can detect such attacks.
21. Etag/Cookies<br/>
    Etag, or Entity tag, is part of HTTP, used for web cache validation. Web server does not need to send a full response if content no change.
    Cookies,or HTTP cookie, web cookie, browser cookie, contains state information from server sending. Used for anthentication, confirmation of user session. **Hacker wants to get your cookie data**
22. Vulnerable Services<br/>
    1) UUCP is one of the most dangerous because unencrypted and poor authentication method. **Never use unencrypted ports**
    2) **ports not defined in /etc/services should be defined in /etc/service.local**
    3) All services within (netstat -a) should be monitored and undocumented services should be disable. **Hacker likes bogus ports**
23. Client/Server Architecture<br/>
    1) **Perform everything possible on the client side** because it'll improve performance, reduce network traffic, reduce network stream compromised.
    2) **TCP over UDP for more secure**
    3) **if complexity is greater than three, make sure each network link is protected**
24. Pseudo terminal process id<br/>
    `$ps -aux | grep dguster`   # output pts/0 pts/2 mean pseudo terminal
25. Hard character (can be printed out) and Soft character (first 32 non-printing characters)
26. Swap --part of disk as memory
27. Character device (Memory), Block device (disk)
28. sda1 --system drive, physical disk a, logical partition 1
29. 3 levels access rights
   ```
   1) kernel level
   2) root level
   3) user level
   ```
30. 3 data locations
   ```
   1) network (most dangerous) #local, fiber, Ethernet, loopback. tcpdump
   2) memory   #cache, shared buffer. dd, xxd
   3) disk     #xxd
   ```
31. layered security
   ```
   1) cloud -> zone -> host
   2) fs -> dir -> file
   ```
32. file formats
   ```
   1) binary format  #faster, encapsulated, encrypted binary
   2) ASCII format
   3) EBCDIC format
   ```
33. Primary data
   ```
   #Hackers really wants
   #Hexdecimal numbers
   #Example: www.sun.com (no), 156.151.59.36 (yes)
   ```
34. CIA (Confidentiality, Integrity, availability)
   ```
   CI: positive relation
   CA: negative relation
   IA: negative relation
   ```
35. 2 tasks of process
   ```
   1) copy data
   2) calculate data
   ```
36. IO   and  NIO  in  java lib
   ```
   IO: stream oriented, blocking IO
   NIO: buffered oriented, Non-blocking IO, Selectors
   Buffered oriented: check if buffer contains all data you need; make sure not to overwrite data you have not processed
   Blocking IO: when a thread invoke read() or write(), the thread is blocked until finished read or write
   Non-blocking IO: thread can do other things even if not finish r or w
   Selectors: a thread can monitor multiple channels
   ```
37. Kerberos and LDAP
   ```
   Kerberos --a network authentication protocol, created by MIT. Protected from against eavesdropping and replay attacks
   Ldap is used to holding authoritative information about accounts;
   Kerberos is used to manage credentials securely.
   ```
38. Java primitive Data type size
   ```
   boolean: default (false), 1 bit
   char:    default (\u0000), Unicode character, 2 Bytes
   byte:    default (0),   1 B
   short:   default (0),   2 B
   int:     default (0),   4 B
   long:    default (0),   8 B
   float:   default (0.0), 4 B
   double:  default (0.0), 8 B
   ```
39. Compression via RLE (run length encoding)
   ```
   013011 = a string of 17 0's and reduction 14 bytes (17 - 3)
   01452b = a string of 43 E's and reduction 40 bytes (43 - 3)
   01: means rle
   45: E
   2b: nubmer of E
   ```
40. Laws
   ```
   Murphy's Law:"Anything that can go wrong will go wrong!"
   Linus's Law:"Given enough eyeballs, all bugs are shallow!"
   Miller's Law:"At any time, we can concerntrat on only approximately seven chunks!"
   Brooks's Law:"Adding personel to a late software project makes it then later!"
   Guster:"We cannot stop hacker, but can slow them down!"
   Guster:"The most robust is the use of file hashes!"
   ```
41. output of time command
   ```
   real time: cpu time + waiting time
   user time: application time
   sys  time: cpu time
   ```