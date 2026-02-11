# Networking Concepts: DNS, IP, Subnets & Ports

## Task 1: DNS – How Names Become IPs

what happens when you type google.com in a browser?

<p> Typing "google.com" initiates a multi-step process in milliseconds - the browser resolves the domain to an IP address via DNS, establishes a secure HTTPS/TLS connection with Google's server (via TCP), sends an HTTP request, receives HTML/CSS/JS files, and renders the webpage, all often passed through security firewalls</p>

Here is the detailed breakdown:
- DNS Lookup: The browser first checks its cache for the IP address of google.com. If not found, it asks the Operating System, which queries DNS servers to translate the domain name into an IP address.
- TCP/IP Connection: The browser establishes a connection with the server using a three-way handshake (SYN, SYN-ACK, ACK) to ensure reliable communication.
- HTTPS/SSL Handshake: A secure connection is established to authenticate the server and encrypt data, ensuring privacy.
- HTTP Request and Response: The browser sends a GET request to the server, which then processes it (often using load balancers to select the best server) and sends back the website's data.
- Rendering the Page: The browser parses the HTML, CSS, and JavaScript files, creating the Document Object Model (DOM) and rendering the final webpage on the screen.

What are these record types? Write one line each:
A, AAAA, CNAME, MX, NS
- A (Address): Maps a hostname directly to an IPv4 address.
- AAAA (Quad-A): Maps a hostname directly to an IPv6 address.
- CNAME (Canonical Name): Maps an alias or subdomain to another domain name rather than an IP address.
- MX (Mail Exchange): Directs email to the appropriate mail server for the domain.
- NS (Name Server): Specifies the authoritative name servers that hold the DNS records for a domain.

Run: dig google.com — identify the A record and TTL from the output

<img width="588" height="311" alt="image" src="https://github.com/user-attachments/assets/655931ce-2880-48c8-b6ce-6f532278c853" />

A - record gives IPv4 address - 172.217.167.238
TTL - Time To Live - 294secs

<hr/>

## Task 2: IP Addressing

What is an IPv4 address? How is it structured? (e.g., 192.168.1.10)

<p>An IPv4 address is a unique numerical label assigned to every device connected to a network that uses the Internet Protocol for communication. It acts like a digital home address, allowing data to be routed correctly between devices on the internet. 
Structure of an IPv4 Address.</p>

<p>An IPv4 address is a 32-bit number. While computers process this as a string of binary digits, it is structured for humans in dotted-decimal notation.</p>

- Octets: The 32 bits are divided into four 8-bit segments called octets or bytes.
- Decimal Range: Each octet can represent a decimal value from 0 to 255.
- Visual Format: The four decimal numbers are separated by periods (e.g., 192.168.1.1). 

- Logical Components
  - Every IPv4 address is split into two logical parts, which are defined by its subnet mask: 
  - Network ID: The leftmost portion that identifies the specific network the device belongs to.
  - Host ID: The rightmost portion that uniquely identifies a specific device (host) within that network. 
- Address Classification
  Historically, IPv4 addresses were divided into five classes (A, B, C, D, and E) based on the first few bits of the first octet: 
  - Class A: Designed for very large networks (e.g., 10.x.x.x).
  - Class B: Intended for medium-sized networks.
  - Class C: Used for small local networks.
  - Class D & E: Reserved for multicast groups and experimental research, respectively. 

Difference between public and private IPs
<img width="2135" height="569" alt="image" src="https://github.com/user-attachments/assets/f2d5fc4c-8397-4b70-b483-3e407d2d3e08" />

What are the private IP ranges?
10.x.x.x, 172.16.x.x – 172.31.x.x, 192.168.x.x

   - 10.x.x.x - Large enterprise networks
   - 172.16.x.x – 172.31.x.x - Medium-sized organizations
   - 192.168.x.x - Home & small office networks

Run: ip addr show — identify which of your IPs are private

<img width="856" height="316" alt="image" src="https://github.com/user-attachments/assets/c0418e6d-0310-4224-8aa2-b509847757c1" />

127.0.0.1/8 - Reserved for local host communication
192.168.29.92/24 - This is a private IP address.

<hr/>

## Task 3: CIDR & Subnetting

What does /24 mean in 192.168.1.0/24

In IP terms, an address like 192.168. 1.0/24 tells us both the address and how to split it into network vs host parts. Here, 192.168. 1.0 is the IP address (in dotted decimal form) and /24 is the CIDR notation (it means the first 24 bits of the 32-bit address are the network portion, leaving 8 bits for hosts).

<img width="1600" height="635" alt="image" src="https://github.com/user-attachments/assets/c304b57f-6dc3-4dc8-aef3-83ad3f723f07" />

How many usable hosts in a /24? A /16? A /28?

Based on standard IPv4 subnetting (using the \(2^{32-\text{CIDR}}-2\) formula), here are the number of usable hosts for each requested CIDR notation. The subtraction of 2 accounts for the network address and the broadcast address. 
 - /24: 254 usable hosts (256 total IPs)
 - /16: 65,534 usable hosts (65,536 total IPs)
 - /28: 14 usable hosts (16 total IPs)

why do we subnet?

Subnetting divides a large, inefficient network into smaller, manageable, and more secure sub-networks (subnets). It boosts performance by reducing broadcast traffic congestion, enhances security through isolation, and optimizes IP address allocation, ensuring efficient use of limited IPv4 resources. 

Key reasons for subnetting include:
  - Improved Network Performance: By breaking large networks into smaller ones, broadcast traffic is restricted, which prevents network congestion and reduces the load on devices.
  - Enhanced Security: Subnets enable segmentation, allowing sensitive devices (like financial servers) to be isolated from the rest of the network, which minimizes the impact of security breaches.
  - Simplified Management & Troubleshooting: Smaller networks are easier to manage, monitor, and troubleshoot compared to one massive network.
  - Efficient IP Address Usage: Subnetting allows for flexible, efficient allocation of IP addresses, which is crucial given the scarcity of IPv4 addresses.
  - Improved Routing Efficiency: Routers use subnets to direct traffic more directly, reducing the distance data travels. 

<img width="625" height="200" alt="image" src="https://github.com/user-attachments/assets/8e0dda12-b2a4-4e3b-9e84-98581851faba" />

<hr/>

## Task 4: Ports – The Doors to Services

What is a port? Why do we need them?

A port is a logical identifier used to distinguish different applications or services on a device, allowing network traffic to reach the correct program.

  - Ports work at the Transport layer, using TCP and UDP to send and receive data between devices.
  - They enable multiple applications to use the network simultaneously without interference.
  - Ports help the operating system route incoming data to the appropriate application.

Document these common ports:

| Port | Service |
|------|---------|
| 22   | SSH     |
| 80   | nginx   |
| 443  | http    |
| 53   | DNS     |
| 3306 | mysql   |
| 6379 | redis   |
| 27017| mongo db|

<img width="1314" height="227" alt="image" src="https://github.com/user-attachments/assets/0b19446b-e5a0-47fe-9681-9af5c384ae86" />

<hr/>

## Task 5: Putting It Together

When you run curl http://localhost:80

  - Protocol HTTP
  - Localhost : Resolve to loopback IP it resolves to 127.0.0.1
  - Port 80 : Apache service

curl (Client URL) is a powerful command-line tool and library for transferring data to or from a server using various protocols (HTTP, HTTPS, FTP, etc.), commonly used for downloading files, testing web APIs, web scraping, and automating network requests from the terminal. It allows developers to send requests (GET, POST) and receive responses, inspecting headers, sending data and handling authentication without needing a web browse


Your app can't reach a database at 10.0.1.50:3306 — what would you check first?

  - `ss -tulpn | grep 3306` - Check if port is open and service is listening.
  - `systemctl status mysql` - Check service status
  - `nc -zv 10.0.1.50 3306` - Check connectivity
  - `journalctl -u mysql` - Check Logs

The very first thing to check is network connectivity between your application and the database server to determine if the issue is a "refused connection" (server down) or a "timeout" (firewall/network block). 

Use the following command from the application server/machine to test this.
`telnet 10.0.1.50 3306 (or nc -zv 10.0.1.50 3306)` 

Here is the prioritized troubleshooting flow:

1. Check Network & Firewall (Most Likely) 
   - Firewall Rules: Ensure the server at 10.0.1.50 has an inbound rule allowing TCP traffic on port 3306.
   - Security Groups (if in cloud): If using AWS RDS, ensure the security group permits inbound access from your app's IP on port 3306.
   - VPN/Routing: If the IP is in a private network, ensure your application has network access to that IP (VPN, VPC peering, etc.). 

2. Check Database Service Status 
   - Is MySQL running? Check if the database service is actually running on 10.0.1.50. If it crashed or is restarting, the connection will be refused.
   - Bind Address: Check the MySQL configuration (my.cnf or my.ini) to ensure bind-address is not set to 127.0.0.1 (which restricts it to localhost). It should be 0.0.0.0 or the specific IP 10.0.1.50. 

3. Verify Connection Details
   - Confirm IP/Port: Double-check that the IP (10.0.1.50) and Port (3306) are correct in your application's connection string. 
