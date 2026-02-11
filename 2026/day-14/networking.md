# Networking Fundamentals

## OSI Model

<p>The Open Systems Interconnection (OSI) model is a conceptual framework, standardized in 1984, that breaks down network communication into seven distinct layers, from physical transmission (Layer 1) to user applications (Layer 7).</p>

The layers are: Application, Presentation, Session, Transport, Network, Data Link, and Physical. 
There are the 7 OSI Model Layers in order from top to bottom.
* Application Layer: The layer closest to the end-user, facilitating software-to-network communication (e.g., HTTP, FTP, SMTP).
* Presentation Layer: Translates, encrypts, and compresses data to ensure compatibility and security between application and network formats.
* Session Layer: Manages (establishes, maintains, and terminates) sessions between computers.
* Transport Layer: Handles end-to-end communication, flow control, segmentation, and error correction (e.g., TCP, UDP).
* Network Layer: Manages logical addressing (IP addresses) and routing, determining the best path for data.
* Data Link Layer: Facilitates node-to-node data transfer, framing, and physical addressing (MAC addresses).
* Physical Layer: Transmits raw binary data (bits) over physical media like cables, switches, and radio waves.

## TCP/IP Model

<p>The TCP/IP model consists of four, sometimes depicted as five, functional layers that standardize network communication. From top to bottom, these layers are the Application, Transport, Internet (Network), and Link (Network Access/Physical) layers.</p>

It facilitates end-to-end data delivery by handling user applications, host-to-host communication, routing, and physical data transmission. 

The Four-Layer Model (Original/Core Model):
* Application Layer: Provides network services to user applications (HTTP, FTP, SMTP, DNS).
* Transport Layer: Manages end-to-end communication, flow control, and error correction using protocols like TCP and UDP.
* Internet Layer (or Network Layer): Handles addressing and routing of packets across independent networks, primarily using IP (Internet Protocol).
* Network Access/Link Layer: Manages the physical transmission of data on a specific network segment (Ethernet, Wi-Fi).

## Protocols
Protocols used in `OSI Model layer`
* Layer 7 - Application: HTTP, HTTPS, SMTP, FTP, DNS, SSH, SNMP, Telnet.
* Layer 6 - Presentation: SSL/TLS, MIME, ASCII, EBCDIC.
* Layer 5 - Session: NetBIOS, RPC, PPTP, SMB, SIP.
* Layer 4 - Transport: TCP (Transmission Control Protocol), UDP (User Datagram Protocol), SCTP.
* Layer 3 - Network: IP (Internet Protocol), ICMP, IGMP, OSPF, ARP.
* Layer 2 - Data Link: Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11), PPP, Switching protocols.
* Layer 1 - Physical: Ethernet (cabling/signaling), USB, Bluetooth, Wi-Fi frequencies.

Protocols used in `TCP/IP Model Layers`
* Application Layer: High-level protocols for user-facing applications.
    * HTTP/HTTPS: Web browsing.
    * FTP: File transfers.
    * SMTP: Email transmission.
    * DNS: Translates domain names to IP addresses.
* Transport Layer: Manages end-to-end communication and data flow.
    * TCP (Transmission Control Protocol): Reliable, connection-oriented, error-checked delivery.
    * UDP (User Datagram Protocol): Fast, connectionless, best-effort delivery (e.g., streaming, gaming).
* Internet Layer (or Network Layer): Handles logical addressing and routing.
    * IP (Internet Protocol): Addresses and forwards packets (IPv4, IPv6).
    * ICMP (Internet Control Message Protocol): Error reporting and diagnostics (e.g., ping).
    * ARP (Address Resolution Protocol): Maps IP to physical addresses.
* Network Access Layer (or Link Layer): Deals with physical transmission over the network medium.
    * Ethernet, Wi-Fi: Standards for local network communication.
 
<img width="1345" height="242" alt="image" src="https://github.com/user-attachments/assets/81c645b8-ae56-4007-acbf-417e2da7f411" />

## Hands-on Checklist 

- **Identity:** `hostname -I` (or `ip addr show`) — note your IP.
- **Reachability:** `ping <target>` — mention latency and packet loss.
- **Path:** `traceroute <target>` (or `tracepath`) — note any long hops/timeouts.
- **Ports:** `ss -tulpn` (or `netstat -tulpn`) — list one listening service and its port.
- **Name resolution:** `dig <domain>` or `nslookup <domain>` — record the resolved IP.
- **HTTP check:** `curl -I <http/https-url>` — note the HTTP status code.
- **Connections snapshot:** `netstat -an | head` — count ESTABLISHED vs LISTEN (rough).

<img width="764" height="464" alt="image" src="https://github.com/user-attachments/assets/84cb1520-d48d-4c0a-b15a-629c55ac8248" />

<img width="1312" height="428" alt="image" src="https://github.com/user-attachments/assets/5da2a145-0b37-4088-9cbe-e3c3543735ac" />

<img width="1350" height="680" alt="image" src="https://github.com/user-attachments/assets/b8554651-1db9-4b7c-810b-f897bff9f565" />

## Mini Task: Port Probe & Interpret
1) Identify one listening port from `ss -tulpn` (e.g., SSH on 22 or a local web app).  
2) From the same machine, test it: `nc -zv localhost <port>` (or `curl -I http://localhost:<port>`).  

<img width="1366" height="498" alt="image" src="https://github.com/user-attachments/assets/2baed8db-68c5-4880-bdf5-60fa69c4599f" />

3) Write one line: is it reachable? If not, what’s the next check? (e.g., service status, firewall).
**If not reachable :** 
- Check service status - `systemctl status ssh`
- Check logs - `journlctl -u ssh`
- Check firewall - `sudo ufw status`

## Reflection

- **Ping** command gives fastest signal if something is broken.
- DNS fails : It runs on application layer if DNS queries don’t resolve, the next logical layer to inspect is 
  the Transport layer (L4) and Internet layer (L3)
  
  -> dig, nslookup, ping, ss -tulpn
- HTTP 500 : It is application layer. Since you got response(500) it means internet and transport layers are fine.
  Check at Application layer.
  
  -> systemctl status service, journalctl -u service, tail -f /var/log/service/error.log
- Follow up checks in real incident :
    * Check firewall (`sudo ufw status`,`sudo iptables -L -n -v`)
    * Service helth check (`systemctl status <service>`)
    * Connectivity test (`curl -I http://<server-ip>:<port>`,`nc -zv <server-ip> <port>`)

