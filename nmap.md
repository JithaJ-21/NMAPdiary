* A ***network*** is a group of devices connected so that they talk to each other and share data. Each device on a network is identified by a uniue address called ***IP***(Internet Protocol).
* All networks connected globally forms the ***internet*** and the wifi network created by your router at home/office forms the ***local wifi network***.
* IPv4 – Most common format: four numbers separated by dots; Each number is between 0–255. e.g: 192.168.1.5
* IPv6 – Newer format for more devices: eight groups of hexadecimal numbers. e.g:2001:0db8:85a3:0000:0000:8a2e:0370:7334
* Public IP: Your device’s address on the internet. Websites use it to know where to send your data.
* Private IP: Your device’s address inside a local network (like your home Wi-Fi). Routers translate private IPs to public IPs using NAT (Network Address Translation).
* Imagine your house: Your room = device; Your appartment building(all rooms connected) = local wifi; room no. = private IP; building lobby(routes mails) = Router; mail = data packets; street address(for outside world) = public IP; the entire city(all buildings connected) = internet--Without IPs, computers wouldn’t know where to send information.
* Before hacking/defending, reconnaissance is important i.e. gathering info. about a network(which systems are alive, discovering hosts inside a network, scan for open ports, identify services running on those ports, detect the OS and version of the machine, also find vulnerabilities using advanced scripts). For this, ***NMAP*** was developed in ***1997***
* ***nmap -h*** can be used to gets all nmap commands and flags
* Basic commands:
  
|SL.No.| purpose | example | what it does|
|---|---|---|---|
|1.|scanning a single target|nmap -Pn scanme.org|created by NMAP creators for practice- scans the server|
|2.|scanning by IP address|nmap 192.168.1.1|scans specific machine inside your local network|
|3.|scanning multiple IPs|nmap 192.168.1.1 192.168.1.50|scans the specified IPs one after the other|
|4.|scanning a range of IPs|nmap 192.168.1.1-50|scans from .1 to .50 in the subnet|
|5.|scanning the whole subnet|nmap 192.168.1.0/24|scans all 254 devices|

* Port scanning:

|SL.No.| scan type | what it does | advantages | disadvantages |
|---|---|---|---|---|
|1.|TCP connect scan (-sT)|fully connects to every port|very reliable|easy to detect by firewalls and IDS(Intrusion Detection System)|
|2.|SYN scan (-sS)|instead of fully connecting, it just sends the SYN packet, waits for response and drops the connection|faster, stealthier, harder to detect|requires root/admin privileges|
|3.|UDP scan (-sU)|not all services use TCP, DNS and SNMP use UDP so this scans that TCP scans miss|finds services that TCP scans miss|slower and noisy|
|4.|FIN scan (-sF)|sends FIN packet to close connections instaed of opening them- some systems don't respond properly which allows to detect open ports quietly|stealthy|cannot reliably distinguish between open and filtered ports|
|5.|XMAS scan (-sX)|sets unusual flags like FIN(finish), URG(urgent), PSH(push) eetc. thaty confuse the system then if port is closed, system replies and if open, it stays silent|slip past simple firewalls|doesn't work well with all systems|
|6.|NULL scan (-sN)|sends packets with no flags-some systems do not know how to respond|very sneaky|unreliable against modern firewalls|

 Nmap Scan Response Master Table:

| Scan Type        | OPEN        | CLOSED             | FILTERED           |
|------------------|-------------|--------------------|--------------------|
| TCP Connect (-sT)| SYN-ACK     | RST                | No response / ICMP |
| TCP SYN (-sS)    | SYN-ACK     | RST                | No response / ICMP |
| UDP (-sU)        | No response | ICMP Unreachable   | No response        |
| FIN (-sF)        | No response | RST                | No response        |
| NULL (-sN)       | No response | RST                | No response        |
| XMAS (-sX)       | No response | RST                | No response        |


* Host discovery:

|SL.No.|scan|purpose|
|---|---|---|
|1.|Ping scan (-sn)|easiest way to discover all live hosts in a network|
|2.|ICMP Echo, Timestamp & Netmask(-PE, -PP, -PM)|used when some firewalls block normal ping but allows other ICMP types|
|3.|ARP scan (-PR)|sends ARP request to all IPs and can't be blocked by a firewall on a LAN|
|4.|skip host discovery (-Pn)|sometimes firewall completely blocks ping requests- in that case, you can skip host discvery and force nmap to treat all hosts as alive; since it scans all hosts(including dead) it takes longer time|
|5.|combine host discovery with port scanning (-sn --open)|shows live hosts with open ports- useful for large network|

* ***ICMP***(Internet Control Message Protocol) is the protocol used by network devices to send control messages(not for transferring data)
* ***ARP***(Address Resolution Protocol) finds the MAC address of a device when IP address of the device is known
* ***IP*** is the logical address whereas ***MAC*** is the physical address used to deliver frames. In simpler words, you know the room no.(IP) but you need to know ther person's name/identity(MAC) to deliver the letter
* ***sudo*** runs a command with root/admin privilege

* To find the exact software running, its version, OS behind it etc.:

|SL.No.|scans|what it does|
|----|----|---|
|1.|Aggressive version detection (-A)|used for aggressive version detection, OS detection, script scanning, trace routes etc.|
|2.|Service and version detection (-sV)|to detect version etc. - if you find any vulnerabilities, you just found an entry point|
|3.|OS detection (-O)|to detect OS by analysing the network|

* Advanced Scanning & Stealth with:
1) ***Decoys*** (ex: nmap -D RND:5 192.168.1.1 - 5 random IPs are scanning along with you thus firewall logs will have no idea on who the real attacker is)
2) ***Fragmentation*** (-f) - nmap fragmets data packets into small chunks to sneak through easily without firewall/IDS detection
3) ***Source port tricks*** (ex: sudo nmap --source-port 53 - makes the firewall believe that it's coming from DNS traffic port 53 whch many fiewalls allow by default)

* ***DNS***(Domain Name System) converts domain name(like google.com) into IP address
* ***Timing templates***

|Timing template option|Name|
|---|---|
|-T0|Paranoid|
|-T1|Sneaky|
|-T2|Polite|
|-T3|Default|
|-T4|Aggressive|
|-T5|Insane|

* As we go from -T0 to -T5, stealth decreases, speed increases
* ***NSE*** i.e. nmap scripting engine transforms nmap into a hacking machine by allowing nmap to run custom scripts written in lua which is a lightweight programming language - to detect vulnerabilities, brute force passwords, exploit services etc.
* -sC - to run default scripts, to enhance scans with extra details by checking for common services, misconfigurations and weak points
* --script script_category - to scan by category like auth-authentication attacks, vuln-vulnerability detection, brute-password attacks, discovery-for information gathering etc.
* We can also run specific scripts (scan by name)- ex: --script=http-title, --script=ftp-anon, --script=smb-vuln* etc.
* Saving the output
1) to a text file -oN
2) in greppable format -oG
3) XML -oX
4) in all formats -oA

Ex: nmap -sV scanme.org -oN results.gnmap  

|format|analogy|
|---|---|
|.txt|story book(easy to read)|
|.gnmap|excel rows(easy to search)|
|.xml|database schema(tools love it)|

* ***Zenmap*** is the official nmap GUI(graphical user interface)
* It does everything that nmap does but in a clean beginner friendly window, perfect for those who want results fast without memorising commands
* sudo apt install zenmap - to install zenmap
* sudo zenmap - to open zenmap
* In zenmap, we type the target name/ip, select scan type and hit 'scan'
* Exploring result tabs- the tabs are:
1) NMAP output - shows row terminal output
2) Ports/hosts - gives a neat table of open ports
3) Topology - shows interactive map of network
4) Host details - shows host status, addresses, OS etc
5) Scans - gives details of the scans done
* On the top left corner of the screen, we have options for comparing results, saving scans etc. tools->compare results; scan->save scan
