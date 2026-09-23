
# 🌐 Chapter 8: Networking Fundamentals

Networking is a core skill for DevOps engineers. It helps us connect servers, troubleshoot application connectivity, manage cloud infrastructure, and understand how applications communicate.

---

## 📑 Table of Contents

1. [Introduction](#-introduction)
2. [OSI Model](#-1-osi-model)
3. [TCP/IP Model](#-2-tcpip-model)
4. [Networking Devices](#-3-networking-devices)
5. [Network Topologies](#-4-network-topologies)
6. [Types of Networks](#-5-types-of-networks)
7. [IP Addressing](#-6-ip-addressing)
8. [Public IP vs Private IP](#-7-public-ip-vs-private-ip)
9. [Subnetting and CIDR](#-8-subnetting-and-cidr)
10. [TCP vs UDP](#-9-tcp-vs-udp)
11. [Important Network Ports](#-10-important-network-ports)
12. [DNS](#-11-dns-domain-name-system)
13. [HTTP and HTTPS](#-12-http-and-https)
14. [MAC Address and ARP](#-13-mac-address-and-arp)
15. [Linux Network Commands](#-14-linux-network-commands)
16. [Network Interfaces](#-15-network-interfaces)
17. [Routing](#-16-routing)
18. [Ping and Connectivity Testing](#-17-ping-and-connectivity-testing)
19. [Traceroute](#-18-traceroute)
20. [SSH](#-19-ssh-secure-shell)
21. [Network Tools](#-20-network-tools)
22. [Firewalls](#-21-firewalls)
23. [Network Troubleshooting](#-22-network-troubleshooting)
24. [DevOps Networking Use Cases](#-23-devops-networking-use-cases)
25. [Hands-On Practice](#-24-hands-on-practice)
26. [Important Interview Questions](#-25-important-interview-questions)
27. [Quick Revision](#-26-quick-revision)
28. [Chapter Checklist](#-chapter-checklist)

---

## 📘 Introduction

### What is Networking?

Networking is the process of connecting computers, servers, and devices to communicate and share data.

### Why Networking is Important in DevOps

- Connect application servers and databases.
- Configure cloud networking.
- Troubleshoot server connectivity.
- Manage ports and firewall rules.
- Configure DNS and domain names.
- Understand load balancers and reverse proxies.
- Secure communication using SSH and HTTPS.
- Monitor application network traffic.

---

## 📗 1. OSI Model

The **OSI (Open Systems Interconnection) model** is a seven-layer conceptual framework used to understand network communication.

| Layer | Name | Description |
|-------|------|-------------|
| 7 | Application | Network services used by applications |
| 6 | Presentation | Data formatting, encryption, compression |
| 5 | Session | Establishes and manages sessions |
| 4 | Transport | End-to-end communication using TCP/UDP |
| 3 | Network | IP addressing and routing |
| 2 | Data Link | Frames, MAC addresses, local network delivery |
| 1 | Physical | Transmission of bits through physical media |

### 🔹 OSI Layers in Simple Language

**Layer 7 – Application**

Provides network services to applications.

Examples:
- HTTP
- DNS
- SMTP

**Layer 6 – Presentation**

Handles data representation, encryption, and compression where applicable.

**Layer 5 – Session**

Manages communication sessions between applications.

**Layer 4 – Transport**

Provides end-to-end transport using protocols such as TCP and UDP.

**Layer 3 – Network**

Handles logical addressing and routing using IP.

**Layer 2 – Data Link**

Handles local network frames and MAC addressing.

**Layer 1 – Physical**

Transmits raw bits through cables, fiber, or wireless signals.

> **Interview Tip:** The OSI model is conceptual. Real protocols may span or combine responsibilities across layers.

---

## 📗 2. TCP/IP Model

The TCP/IP model represents the protocol architecture used in the Internet.

| TCP/IP Layer | OSI Mapping | Examples |
|--------------|-------------|----------|
| Application | Layers 5–7 (commonly mapped) | HTTP, DNS, SSH |
| Transport | Layer 4 | TCP, UDP |
| Internet | Layer 3 | IP, ICMP |
| Network Access | Layers 1–2 | Ethernet, Wi-Fi |

### TCP/IP vs OSI

| Parameter | OSI | TCP/IP |
|-----------|-----|--------|
| Layers | 7 | Commonly 4 |
| Purpose | Conceptual reference model | Internet protocol architecture |
| Usage | Learning and troubleshooting | Practical networking |
| Developed By | ISO | DARPA / Internet protocol community |

---

## 🔌 3. Networking Devices

### 1. Hub

- Operates at the Physical Layer (Layer 1).
- Sends incoming signals to all ports.
- Does not intelligently filter frames.

### 2. Bridge

- Operates at Layer 2.
- Connects network segments.
- Uses MAC addresses to filter traffic.

### 3. Switch

- Primarily operates at Layer 2.
- Forwards Ethernet frames based on MAC addresses.
- Some switches also provide Layer 3 routing.

### 4. Router

- Connects different IP networks.
- Forwards packets using routing information.
- Commonly operates at Layer 3.

### 5. Firewall

- Controls network traffic according to security rules.
- Can operate at different layers depending on its capabilities.

### 6. Load Balancer

- Distributes client traffic across backend servers.
- Improves availability and scalability.
- Can operate at Layer 4 or Layer 7.

---

## 🕸️ 4. Network Topologies

| Topology | Description |
|----------|-------------|
| Bus | Devices share a common backbone |
| Star | Devices connect to a central switch or device |
| Ring | Devices form a circular connection |
| Mesh | Devices have multiple interconnections |
| Hybrid | Combination of different topologies |

### DevOps Relevance

Modern cloud environments commonly use logical network designs with subnets, routing, firewalls, and load balancers rather than relying only on traditional physical topologies.

---

## 🌍 5. Types of Networks

| Network Type | Description |
|--------------|-------------|
| LAN | Local Area Network |
| MAN | Metropolitan Area Network |
| WAN | Wide Area Network |
| WLAN | Wireless Local Area Network |
| VPN | Encrypted or authenticated private network connection over another network |

### Examples

- LAN: Office network
- WAN: Internet-connected company branches
- WLAN: Wi-Fi network
- VPN: Secure remote access to internal resources

---

## 🔢 6. IP Addressing

An **IP address** identifies a network interface for communication using the Internet Protocol.

### IPv4

- 32-bit address.
- Written in dotted-decimal format.
- Example: `192.168.1.10`

### IPv6

- 128-bit address.
- Written in hexadecimal notation.
- Example: `2001:db8::1`

### Important Terms

| Term | Meaning |
|------|---------|
| Network Address | Identifies the subnet |
| Host Address | Identifies an address within the subnet |
| Subnet Mask | Separates network and host portions in IPv4 |
| Default Gateway | Router used to reach other networks |
| Broadcast Address | IPv4 address used to reach all hosts in a subnet where broadcast is supported |

### Example

```text
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
CIDR: /24
```

For a typical IPv4 `/24` subnet:

```text
Network Address:   192.168.1.0
Usable Host Range: 192.168.1.1 - 192.168.1.254
Broadcast Address: 192.168.1.255
```

> **Note:** The usable host range depends on the subnet size and whether the network uses traditional IPv4 subnet conventions.

---

## 🌐 7. Public IP vs Private IP

### Private IPv4 Ranges

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

### Difference Table

| Parameter | Public IP | Private IP |
|-----------|-----------|------------|
| Scope | Publicly routable on the Internet | Private network scope |
| Uniqueness | Globally coordinated addressing | Can be reused in separate networks |
| Common Usage | Public-facing services | Internal communication |
| Internet Access | May be directly reachable subject to routing/firewall rules | Usually requires NAT or another connectivity mechanism for Internet access |
| Example | `8.8.8.8` | `192.168.1.10` |

> **Security Note:** A private IP is not automatically secure. Security depends on routing, firewalls, access controls, and system configuration.

---

## 🔢 8. Subnetting and CIDR

**Subnetting** divides a larger IP network into smaller networks.

**CIDR (Classless Inter-Domain Routing)** expresses network prefixes using a slash notation.

### Common CIDR Examples

| CIDR | IPv4 Addresses |
|------|----------------|
| `/8` | 16,777,216 |
| `/16` | 65,536 |
| `/24` | 256 |
| `/25` | 128 |
| `/26` | 64 |
| `/27` | 32 |
| `/28` | 16 |
| `/30` | 4 |

> The address counts include network and broadcast addresses for traditional IPv4 subnets.

### Example: /24

```text
Network: 192.168.1.0/24
Total Addresses: 256
Typical Usable Hosts: 254
```

### Example: /26

```text
Network: 192.168.1.0/26
Total Addresses: 64
Typical Usable Hosts: 62
```

### DevOps Use Case

A cloud VPC may be divided into:

```text
VPC: 10.0.0.0/16

Public Subnet:  10.0.1.0/24
Private Subnet: 10.0.2.0/24
Database Subnet: 10.0.3.0/24
```

> **Note:** A subnet is considered public or private based on its routing and access configuration, not simply its IP address range.

---

## 🚦 9. TCP vs UDP

### TCP (Transmission Control Protocol)

- Connection-oriented.
- Provides reliable, ordered byte-stream delivery.
- Uses acknowledgements and retransmission mechanisms.
- Includes congestion and flow control.

### UDP (User Datagram Protocol)

- Connectionless transport protocol.
- Does not provide built-in reliable, ordered delivery.
- Has lower protocol overhead.
- Applications can implement their own reliability mechanisms.

### Difference Table

| Parameter | TCP | UDP |
|-----------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Built-in reliable delivery | No built-in delivery guarantee |
| Ordering | Provides ordered byte stream | No built-in ordering |
| Retransmission | Supported by protocol | Not built in |
| Overhead | Higher | Lower |
| Data Type | Byte stream | Datagrams |
| Common Uses | HTTP/1.1, HTTPS over TCP, SSH | DNS, VoIP, streaming, QUIC transport |
| Speed | Depends on network and protocol behavior | Often useful for low-overhead communication |

> **Important:** DNS can use both UDP and TCP. Modern HTTP/3 uses QUIC, which runs over UDP.

---

## 🔢 10. Important Network Ports

A port identifies a service endpoint associated with a transport protocol.

| Port | Protocol / Service | Common Usage |
|------|--------------------|--------------|
| 20/21 | FTP | File Transfer Protocol |
| 22 | SSH | Secure remote access |
| 23 | Telnet | Remote access (insecure) |
| 25 | SMTP | Email transfer |
| 53 | DNS | Domain name resolution |
| 67/68 | DHCP | IPv4 address configuration |
| 80 | HTTP | Web traffic |
| 110 | POP3 | Email retrieval |
| 123 | NTP | Time synchronization |
| 143 | IMAP | Email retrieval |
| 443 | HTTPS | Encrypted web traffic |
| 3306 | MySQL | Database |
| 5432 | PostgreSQL | Database |
| 6379 | Redis | In-memory data store |
| 8080 | Common alternate HTTP port | Application testing |
| 27017 | MongoDB | Database |

> Port numbers are conventions, not guarantees. Services can be configured to use different ports.

### Check Whether a Port is Listening

```bash
sudo ss -ltnp 'sport = :80'
```

---

## 🌍 11. DNS (Domain Name System)

DNS translates domain names into IP addresses and other records.

### Example

```text
Domain: example.com
IP:     93.184.216.34
```

When a user visits a website, DNS helps the client locate the destination.

### Common DNS Record Types

| Record | Purpose |
|--------|---------|
| A | Maps a domain to an IPv4 address |
| AAAA | Maps a domain to an IPv6 address |
| CNAME | Alias for another domain name |
| MX | Mail exchange server |
| NS | Authoritative name server |
| TXT | Text information, often verification or policy data |
| PTR | Reverse DNS lookup |

### DNS Lookup Commands

```bash
nslookup example.com
```

```bash
dig example.com
```

```bash
dig example.com A
```

```bash
dig example.com MX
```

### DevOps Use Case

DNS is used for:

- Domain routing
- Application endpoints
- Service discovery
- Load balancer names
- Cloud-hosted applications

---

## 🔒 12. HTTP and HTTPS

### HTTP

HTTP (Hypertext Transfer Protocol) is an application-layer protocol used for communication between clients and servers.

### HTTPS

HTTPS is HTTP protected by TLS encryption.

### Common HTTP Methods

| Method | Purpose |
|--------|---------|
| GET | Retrieve data |
| POST | Submit or create data |
| PUT | Replace a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |
| HEAD | Retrieve headers without the response body |

### Common HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 301 | Permanent redirect |
| 302 | Temporary redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |
| 502 | Bad Gateway |
| 503 | Service Unavailable |
| 504 | Gateway Timeout |

### Test an HTTP Endpoint

```bash
curl -I https://example.com
```

### Check Response Details

```bash
curl -v https://example.com
```

### DevOps Use Case

- Test application health endpoints.
- Troubleshoot HTTP errors.
- Verify reverse proxy behavior.
- Check TLS connections.
- Validate deployment endpoints.

---

## 🖧 13. MAC Address and ARP

### MAC Address

A MAC address is a link-layer address used for local network communication.

Example:

```text
00:1A:2B:3C:4D:5E
```

### ARP

**ARP (Address Resolution Protocol)** resolves IPv4 addresses to link-layer addresses on a local network.

IPv6 uses Neighbor Discovery instead of ARP.

### View Neighbor Information

```bash
ip neigh
```

### Display ARP Cache (if available)

```bash
arp -n
```

> The `arp` command may be unavailable by default. `ip neigh` is the preferred modern Linux command.

---

## 🛠️ 14. Linux Network Commands

### 1. Check IP Address

```bash
ip addr
```

Short form:

```bash
ip a
```

### 2. Show Network Interfaces

```bash
ip link
```

### 3. Show Routing Table

```bash
ip route
```

### 4. Check DNS Resolution

```bash
nslookup google.com
```

### 5. Test Connectivity

```bash
ping -c 4 google.com
```

### 6. Display Listening Ports

```bash
ss -tuln
```

### 7. Display Listening Ports with Process Information

```bash
sudo ss -tulnp
```

### 8. Show Network Statistics

```bash
ip -s link
```

### 9. Display Hostname

```bash
hostname
```

### 10. Show Fully Qualified Hostname

```bash
hostname -f
```

### 11. Check DNS Configuration

```bash
cat /etc/resolv.conf
```

> `/etc/resolv.conf` may be managed by NetworkManager, systemd-resolved, or another network management system.

---

## 🌐 15. Network Interfaces

A network interface connects a system to a network.

Examples:

- `eth0` – Ethernet interface
- `ens33` – Common predictable interface name
- `lo` – Loopback interface
- `wlan0` – Wireless interface on some systems

### Show All Interfaces

```bash
ip link show
```

### Show a Specific Interface

```bash
ip addr show eth0
```

### Bring an Interface Up

```bash
sudo ip link set eth0 up
```

### Bring an Interface Down

```bash
sudo ip link set eth0 down
```

> Replace `eth0` with the actual interface name on your system. Changes made with `ip` may not persist after reboot.

---

## 🚦 16. Routing

Routing determines how packets travel from one network to another.

### View Routing Table

```bash
ip route
```

### Example Output

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link
```

### Important Terms

| Term | Meaning |
|------|---------|
| Default Route | Route used when no more specific route matches |
| Gateway | Next-hop device for reaching another network |
| Routing Table | Rules used to select packet routes |
| Destination | Network or host being reached |

### DevOps Use Case

Routing is important when:

- Connecting public and private subnets.
- Configuring cloud VPC routing.
- Accessing databases across networks.
- Troubleshooting unreachable services.

---

## 📡 17. Ping and Connectivity Testing

`ping` uses ICMP Echo Request and Echo Reply messages for connectivity testing.

### Test Localhost

```bash
ping -c 4 127.0.0.1
```

### Test a Server

```bash
ping -c 4 192.168.1.1
```

### Test a Domain

```bash
ping -c 4 google.com
```

### Important Note

A failed ping does not always mean a server is down. ICMP may be blocked by a firewall or disabled.

For application testing, check the specific port and service.

---

## 🛣️ 18. Traceroute

Traceroute helps identify network hops between your machine and a destination.

### Install (if required)

```bash
sudo apt install traceroute
```

### Run Traceroute

```bash
traceroute google.com
```

### Alternative

```bash
tracepath google.com
```

### DevOps Use Case

- Investigate routing problems.
- Identify network hops.
- Troubleshoot latency.
- Understand the path to a remote server.

> Some hops may not respond to traceroute probes. This does not necessarily indicate a failure.

---

## 🔐 19. SSH (Secure Shell)

SSH is used to securely access remote systems.

### Connect to a Remote Server

```bash
ssh username@server-ip
```

### Connect Using a Specific Port

```bash
ssh -p 2222 username@server-ip
```

### Connect Using a Private Key

```bash
ssh -i ~/.ssh/id_rsa username@server-ip
```

### Copy Files to a Remote Server

```bash
scp file.txt username@server-ip:/tmp/
```

### Copy a Directory

```bash
scp -r project/ username@server-ip:/opt/
```

### Generate an SSH Key Pair

```bash
ssh-keygen -t ed25519
```

### Test SSH Verbosely

```bash
ssh -v username@server-ip
```

### Common SSH Troubleshooting

- Check server IP address.
- Verify port 22 is reachable.
- Check firewall rules.
- Verify username and permissions.
- Confirm the SSH service is running.
- Check private key permissions.

```bash
chmod 600 ~/.ssh/id_ed25519
```

> **Security Best Practice:** Use SSH keys, protect private keys, and avoid exposing SSH unnecessarily to the public Internet.

---

## 🔧 20. Network Tools

### 1. Netcat (nc)

Used for testing TCP/UDP connections and transferring data in controlled environments.

#### Test a TCP Port

```bash
nc -vz example.com 443
```

#### Listen on a TCP Port (Testing)

```bash
nc -l 8080
```

> Run network listeners only on systems and ports you are authorized to use.

---

### 2. Curl

Used to make HTTP requests and test endpoints.

```bash
curl -I https://example.com
```

```bash
curl -s https://example.com
```

---

### 3. Wget

Used for retrieving files over supported protocols.

```bash
wget https://example.com/file.txt
```

---

### 4. Dig

Used for DNS queries.

```bash
dig google.com
```

```bash
dig +short google.com
```

---

### 5. Lsof

Used to identify open files and network connections.

```bash
sudo lsof -i :8080
```

---

### 6. Tcpdump

A command-line packet capture and analysis tool.

```bash
sudo tcpdump -i eth0
```

Capture traffic on port 80:

```bash
sudo tcpdump -i eth0 port 80
```

> Packet captures may contain sensitive information. Capture traffic only when authorized.

---

## 🔥 21. Firewalls

A firewall controls network traffic using configured rules.

### Common Linux Firewall Tools

- `ufw` – Uncomplicated Firewall
- `firewalld` – Firewall management service
- `nftables` – Linux packet filtering framework
- `iptables` – Legacy and compatibility firewall tooling

### UFW Examples (Ubuntu)

#### Check Firewall Status

```bash
sudo ufw status
```

#### Allow SSH

```bash
sudo ufw allow 22/tcp
```

#### Allow HTTP

```bash
sudo ufw allow 80/tcp
```

#### Allow HTTPS

```bash
sudo ufw allow 443/tcp
```

#### Enable Firewall

```bash
sudo ufw enable
```

#### Delete a Rule

```bash
sudo ufw delete allow 80/tcp
```

> **Security Warning:** Ensure your SSH access is allowed before enabling a firewall on a remote server. Otherwise, you may lock yourself out.

### DevOps Firewall Best Practices

- Allow only required ports.
- Restrict source IP ranges when possible.
- Avoid exposing databases publicly.
- Use security groups in cloud environments.
- Review firewall rules regularly.
- Follow the principle of least privilege.

---

## 🛠️ 22. Network Troubleshooting

### Problem 1: Server Cannot Access the Internet

#### Step 1: Check IP Address

```bash
ip addr
```

#### Step 2: Check Default Route

```bash
ip route
```

#### Step 3: Test Gateway

```bash
ping -c 4 192.168.1.1
```

#### Step 4: Test Public IP Connectivity

```bash
ping -c 4 8.8.8.8
```

#### Step 5: Test DNS

```bash
getent hosts google.com
```

#### Step 6: Test HTTPS

```bash
curl -I https://google.com
```

---

### Problem 2: Application Port is Not Reachable

#### Check if service is listening

```bash
sudo ss -ltnp 'sport = :8080'
```

#### Check local connectivity

```bash
curl -v http://127.0.0.1:8080
```

#### Check firewall

```bash
sudo ufw status
```

#### Check service status

```bash
sudo systemctl status application.service
```

---

### Problem 3: DNS Resolution Failure

#### Check DNS resolution

```bash
dig google.com
```

#### Check resolver configuration

```bash
cat /etc/resolv.conf
```

#### Test using getent

```bash
getent hosts google.com
```

#### Test direct connectivity

```bash
ping -c 4 8.8.8.8
```

> If IP connectivity works but DNS resolution fails, investigate DNS configuration and resolver availability.

---

### Problem 4: SSH Connection Refused

#### Check the SSH port

```bash
nc -vz server-ip 22
```

#### Check SSH service on the server

```bash
sudo systemctl status ssh
```

#### Check firewall rules

```bash
sudo ufw status
```

#### Run SSH in verbose mode

```bash
ssh -v username@server-ip
```

---

### Problem 5: Connection Timed Out

Possible reasons:

- Firewall dropping traffic.
- Incorrect routing.
- Server unavailable.
- Wrong IP address.
- Security group or network ACL restrictions.
- Service not accessible from the source network.

**Troubleshooting approach:**

1. Verify destination IP and port.
2. Check routing.
3. Test connectivity from the source.
4. Check firewall and cloud security rules.
5. Check whether the service is listening.
6. Review application and network logs.

---

## 🚀 23. DevOps Networking Use Cases

### 🔹 Use Case 1: Check Application Health

```bash
curl -I http://localhost:8080
```

Used to verify whether an HTTP service responds.

---

### 🔹 Use Case 2: Check a Remote Server Port

```bash
nc -vz server-ip 22
```

Used to test TCP connectivity to an SSH port.

---

### 🔹 Use Case 3: Find a Process Using a Port

```bash
sudo lsof -i :8080
```

Used to identify which process is using port 8080.

---

### 🔹 Use Case 4: Troubleshoot DNS

```bash
dig api.example.com
```

Used to inspect DNS resolution.

---

### 🔹 Use Case 5: Monitor Network Traffic

```bash
sudo tcpdump -i eth0 port 443
```

Used to capture authorized traffic for troubleshooting.

---

### 🔹 Use Case 6: Access a Cloud Server

```bash
ssh -i ~/.ssh/cloud-key.pem ubuntu@server-ip
```

Used to connect to a cloud VM using an SSH private key.

> Ensure the key has appropriate permissions and the server allows SSH from your source.

---

## 🧪 24. Hands-On Practice

### Task 1: Display IP Address

```bash
ip a
```

### Task 2: Display Routing Table

```bash
ip route
```

### Task 3: Check DNS Resolution

```bash
nslookup google.com
```

### Task 4: Test Internet Connectivity

```bash
ping -c 4 8.8.8.8
```

### Task 5: Test Website

```bash
curl -I https://example.com
```

### Task 6: Display Listening Ports

```bash
ss -tuln
```

### Task 7: Find Port 22 Usage

```bash
sudo lsof -i :22
```

### Task 8: Test a TCP Connection

```bash
nc -vz google.com 443
```

### Task 9: View Neighbor Table

```bash
ip neigh
```

### Task 10: Display Hostname

```bash
hostname
```

---

## 🎯 25. Important Interview Questions

### 🔹 Basic Networking Questions

1. What is networking?
2. Explain the OSI model.
3. Explain the TCP/IP model.
4. What is the difference between OSI and TCP/IP?
5. What is an IP address?
6. What is the difference between IPv4 and IPv6?
7. What is a MAC address?
8. What is the difference between a switch and a router?
9. What is a default gateway?
10. What is the difference between public and private IP addresses?

### 🔹 TCP/IP Questions

11. What is the difference between TCP and UDP?
12. What is a TCP three-way handshake?
13. What is the difference between a port and an IP address?
14. What is DNS?
15. What are A, AAAA, CNAME, and MX records?
16. What is HTTP vs HTTPS?
17. What are common HTTP status codes?
18. What is NAT?
19. What is subnetting?
20. What is CIDR?

### 🔹 Linux Networking Questions

21. How do you check the IP address in Linux?
22. How do you check the routing table?
23. How do you check listening ports?
24. How do you find which process is using port 8080?
25. How do you test DNS resolution?
26. How do you test connectivity to a remote server?
27. What is the difference between `ping` and `curl`?
28. How do you troubleshoot SSH connection issues?
29. How do you check firewall rules?
30. How do you monitor network traffic in Linux?

### 🔹 DevOps Scenario-Based Questions

31. An application is running but users cannot access it. How do you troubleshoot?
32. Your server has Internet connectivity by IP but not by domain name. What could be wrong?
33. A website returns HTTP 502. What would you check?
34. How do you troubleshoot a connection timeout?
35. How do you check if a service is listening on port 8080?
36. How do you connect to a private cloud server?
37. Why should databases not be exposed publicly?
38. How do security groups differ from Linux firewalls?
39. How would you troubleshoot connectivity between an application and database server?
40. How do you verify whether a DNS record points to the correct server?
41. What is the difference between a public and private subnet?
42. What happens when a client accesses an HTTPS website?
43. How would you investigate high network latency?
44. How do you check whether a firewall is blocking traffic?
45. How do you securely transfer files between Linux servers?

---

## 📝 26. Quick Revision

| Command | Purpose |
|---------|---------|
| `ip a` | Display IP addresses |
| `ip link` | Display network interfaces |
| `ip route` | Display routing table |
| `ip neigh` | Display neighbor information |
| `ping` | Test ICMP connectivity |
| `traceroute` | Trace network path |
| `tracepath` | Trace network path |
| `ss -tuln` | Display listening sockets |
| `ss -tulnp` | Display listening sockets with processes |
| `nslookup` | DNS lookup |
| `dig` | DNS query |
| `curl` | Test HTTP and other supported requests |
| `wget` | Download files |
| `ssh` | Secure remote access |
| `scp` | Secure file transfer |
| `nc` | Test network connections |
| `lsof -i` | Show network-related open files |
| `tcpdump` | Capture network packets |
| `ufw status` | Check UFW firewall status |

---

## 🎯 Key DevOps Concepts to Remember

- **IP Address:** Identifies a network interface at the IP layer.
- **MAC Address:** Link-layer address used for local network communication.
- **DNS:** Resolves domain names into IP addresses and other records.
- **TCP:** Reliable, ordered byte-stream transport.
- **UDP:** Connectionless datagram transport without built-in reliability.
- **Port:** Identifies a service endpoint for a transport protocol.
- **Subnet:** A logical division of an IP network.
- **CIDR:** Represents network prefixes using slash notation.
- **Routing:** Determines where packets should be forwarded.
- **Firewall:** Controls network traffic based on rules.
- **SSH:** Secure remote access protocol.
- **HTTP/HTTPS:** Application-layer web communication protocols.

---

## 🚀 Practical DevOps Project Idea

### 🔧 Linux Network Monitoring Script

Create a Bash script to collect basic network information.

```bash
#!/bin/bash

echo "===== Linux Network Monitoring ====="

echo "Hostname: $(hostname)"
echo "Date: $(date)"

echo ""
echo "===== IP Address ====="
ip -br addr

echo ""
echo "===== Routing Table ====="
ip route

echo ""
echo "===== DNS Test ====="
getent hosts google.com

echo ""
echo "===== Connectivity Test ====="
ping -c 2 8.8.8.8

echo ""
echo "===== Listening Ports ====="
ss -tuln

echo ""
echo "===== Network Statistics ====="
ip -s link
```

### Save the Script

```bash
nano network-monitor.sh
```

### Make It Executable

```bash
chmod +x network-monitor.sh
```

### Run the Script

```bash
./network-monitor.sh
```

### What You Learn

- Bash scripting
- Linux networking commands
- IP addressing
- Routing
- Connectivity testing
- Port monitoring
- Basic troubleshooting

---

## ✅ Chapter Checklist

- [ ] Understand OSI model
- [ ] Understand TCP/IP model
- [ ] Learn networking devices
- [ ] Understand IPv4 and IPv6
- [ ] Learn public and private IPs
- [ ] Practice subnetting and CIDR
- [ ] Understand TCP vs UDP
- [ ] Memorize important ports
- [ ] Understand DNS
- [ ] Learn HTTP and HTTPS
- [ ] Practice Linux networking commands
- [ ] Understand routing
- [ ] Practice SSH
- [ ] Learn firewall basics
- [ ] Practice network troubleshooting
- [ ] Complete the network monitoring project
- [ ] Practice scenario-based interview questions

---

## 🔗 Useful Resources

- [Linux ip command documentation](https://man7.org/linux/man-pages/man8/ip.8.html)
- [Linux ss command documentation](https://man7.org/linux/man-pages/man8/ss.8.html)
- [Linux curl documentation](https://curl.se/docs/)
- [Linux networking manual pages](https://man7.org/linux/man-pages/)
- [MDN HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [Cloud Networking Documentation - AWS](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)

**Next Chapter:** Linux File Permissions and Ownership 🔐
