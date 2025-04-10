# Network

## OSI Model

| OSI Model Layer | Data Unit | Protocols | Devices |
|-----------------|-----------|-----------|---------|
| **Application Layer** (network process to application) | Data | DNS, DHCP, NTP, SNMP, FTP, SSH, Telnet, HTTP, POP3 | Web servers, mail servers, browsers, mail clients |
| **Presentation Layer** (data representation and encryption) | Data | Same as above (handles translation, encryption, etc.) | Same as above |
| **Session Layer** (interhost communication) | Data | Same as above (manages sessions between devices) | Same as above |
| **Transport Layer** (end-to-end connections and reliability) | Segments | TCP, UDP | Gateways, Load Balancers |
| **Network Layer** (path determination & logical addressing (IP)) | Packets | IP, ICMP, IGMP | Routers, Firewalls, Layer 3 Switches |
| **Data Link Layer** (physical addressing (MAC & LLC)) | Frames | ARP, MAC, RARP | Bridges, Layer 2 Switches |
| **Physical Layer** (media, signal and binary transmission) | Bits | Ethernet, Token Ring, DSL, ISDN, etc. | Hubs, Cables, Network Interface Cards |

## Switches

Switches facilitate the sharing of resources by connecting devices such as computers, printers, and servers within a local area network (LAN).  
They operate at the **Data Link Layer (Layer 2)** of the OSI model (some switches also operate at Layer 3) and use **MAC addresses** to forward data to the correct destination within the same network.

## Routers

Routers connect multiple networks together and route data between them.  
They operate at the **Network Layer (Layer 3)** of the OSI model and use **IP addresses** to determine the best path for data to travel across different networks, including LANs, WANs, and the internet.

## Private Network IP Ranges

Private IP addresses are reserved for internal use within private networks and are not routable on the public internet.  
These ranges are defined by [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918).

| Class | IP Range | Subnet Mask |
|-------|----------|-------------|
| A     | 10.0.0.0 – 10.255.255.255 | 255.0.0.0 (/8) |
| B     | 172.16.0.0 – 172.31.255.255 | 255.240.0.0 (/12) |
| C     | 192.168.0.0 – 192.168.255.255 | 255.255.0.0 (/16) |

## Protocols

Each program or service running in a computer communicates over a **unique port number**.
A typical computer system has port numbers ranging from **0 to 65535**, with:

- **0–1023**: Well-known ports (used by standard protocols like HTTP, FTP, SSH)
- **1024–49151**: Registered ports
- **49152–65535**: Dynamic or private ports

tomcat operates on port 8080, mysql on 3306, rabbitmq on 5672, nginx on 80 (http), 443 (https), and ssh on 22.

## TCP/IP Protocol Suite vs OSI Model

| OSI Model Layer     | Corresponding TCP/IP Layer   |
|---------------------|------------------------------|
| Application Layer    | Application Layer            |
| Presentation Layer   | Application Layer            |
| Session Layer        | Application Layer            |
| Transport Layer      | Transport Layer              |
| Network Layer        | Internet Layer               |
| Data Link Layer      | Network Interface Layer      |
| Physical Layer       | Network Interface Layer      |

## 🧰 Common Network Commands

| Command | Description |
|--------|-------------|
| `ifconfig` | Lists network interfaces (older tool, deprecated on newer systems — replaced by `ip`) |
| `ip addr show` | Displays all NICs along with their IP addresses (modern replacement for `ifconfig`) |
| `ping <host>` | Tests basic connectivity between two systems by sending ICMP echo requests |
| `traceroute <host>` / `tracert <host>` (Windows) | Traces the route (hops) packets take to a destination — useful for identifying latency or dropped connections |
| `netstat -antp` | Shows all active TCP connections along with associated process IDs |
| `netstat -ntulp` | Displays both open TCP and UDP ports on the local machine |
| `ss -ntulp` | Faster, more modern tool to view listening ports and sockets (recommended over `netstat`) |
| `nmap <target>` | Scans open ports on a remote system (requires installation; useful for auditing accessible services) |
| `dig <domain>` | DNS lookup utility — helpful for troubleshooting name resolution issues |
| `nslookup <domain>` | Older DNS tool; still useful, especially on Windows systems |
| `route` / `ip route show` | Displays the routing table and default gateways for each interface |
| `arp` | Shows or manipulates the system's ARP cache (IP-to-MAC address mappings) |
| `mtr <host>` | Combines `ping` and `traceroute` to give real-time packet loss and latency info |
| `telnet <host> <port>` | Tests if a specific port is open on a remote host (e.g., `telnet example.com 80`) |
