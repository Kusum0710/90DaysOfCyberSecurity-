# 🌐 Day 1: The OSI Model — The Universal Language of Networking

> *"The OSI model isn't just theory — it's the diagnostic framework every network engineer, security analyst, and pentester uses to pinpoint problems in seconds."*

---

## 🎯 Learning Objectives

By the end of today, you will:
- [x] Understand the OSI model as a **conceptual framework** (not a protocol suite)
- [x] Name all 7 layers in order (bottom-up & top-down)
- [x] Map real protocols, devices, and troubleshooting scenarios to each layer
- [x] Use Wireshark to visualize how a single packet spans multiple OSI layers
- [x] Apply the OSI model as a diagnostic tool for real-world troubleshooting

---

## 🧠 Core Concept: What the OSI Model Actually Is

```
┌─────────────────────────────────────────────────────────┐
│  ⚠️  MYTH BUSTER                                         │
│  The OSI model is NOT a protocol suite like TCP/IP.     │
│  It is a CONCEPTUAL FRAMEWORK that describes HOW        │
│  data moves across networks — regardless of protocol.   │
│  TCP/IP, HTTP, FTP, DNS — they ALL fit within OSI.      │
└─────────────────────────────────────────────────────────┘
```

**Why it matters:**
- Provides a **universal vocabulary** for IT professionals
- Enables **layer-specific troubleshooting** (e.g., "This is a Layer 2 issue, not Layer 3")
- Helps security analysts understand **where attacks occur** and **where to defend**
- Each layer can host **dozens or hundreds** of different protocols

---

## 🏗️ The 7 Layers of the OSI Model

### 📊 Visual Overview

```
┌──────────────────────────────────────────────────────────────┐
│  LAYER 7 │  APPLICATION   │ 👤 User-facing protocols        │
│          │                │ HTTP, HTTPS, FTP, DNS, POP3     │
├──────────────────────────────────────────────────────────────┤
│  LAYER 6 │  PRESENTATION  │ 🔐 Format, encrypt, encode      │
│          │                │ SSL/TLS, JPEG, ASCII, MPEG      │
├──────────────────────────────────────────────────────────────┤
│  LAYER 5 │  SESSION       │ 🔗 Manage connections           │
│          │                │ NetBIOS, RPC, PPTP, tunneling   │
├──────────────────────────────────────────────────────────────┤
│  LAYER 4 │  TRANSPORT     │ 📦 End-to-end delivery          │
│          │                │ TCP (reliable) vs UDP (fast)     │
├──────────────────────────────────────────────────────────────┤
│  LAYER 3 │  NETWORK       │ 🌐 Logical addressing & routing │
│          │                │ IP, ICMP, routers, subnet masks   │
├──────────────────────────────────────────────────────────────┤
│  LAYER 2 │  DATA LINK     │ 🔌 Physical addressing (MAC)    │
│          │                │ Ethernet, switches, MAC/EUI-48   │
├──────────────────────────────────────────────────────────────┤
│  LAYER 1 │  PHYSICAL      │ ⚡ Raw signals on wire/air      │
│          │                │ Cables, fiber, wireless, hubs     │
└──────────────────────────────────────────────────────────────┘
```

### 🧠 Memory Aids

| Direction | Mnemonic | Layers |
|:---------:|:---------|:-------|
| **Top → Bottom** | *"**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing"* | Application → Physical |
| **Bottom → Top** | *"**P**lease **D**o **N**ot **T**hrow **S**ausage **P**izza **A**way"* | Physical → Application |

---

## 🔍 Layer-by-Layer Deep Dive

### ⚡ Layer 1: Physical Layer

> *"If you can't touch it, it's not Layer 1."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Transmission of raw electrical, optical, or radio signals |
| **What lives here** | Cables (Cat5e/Cat6/Cat7), fiber optics, wireless antennas, hubs, repeaters |
| **No complex protocols** | Just signals — 1s and 0s on the medium |
| **PDU** | **Bits** |

**🔧 Troubleshooting at Layer 1:**
- Test cables with a cable tester
- Check for bent pins, damaged insulation
- Detect wireless interference (microwave ovens, Bluetooth)
- Run loopback tests on NICs
- Verify LED indicators on switches/ports

```
⚠️ COMMON LAYER 1 ISSUES:
   • Cable unplugged or damaged
   • Wrong cable type (straight-through vs crossover)
   • Port disabled administratively
   • Wireless signal blocked by walls/metal
   • NIC hardware failure
```

---

### 🔌 Layer 2: Data Link Layer

> *"The MAC address layer — where switches make magic happen."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Direct device-to-device communication on the SAME network segment |
| **Addressing** | **MAC addresses** (EUI-48 / EUI-64) — burned into NIC hardware |
| **Key device** | **Switches** — forward frames based on destination MAC |
| **PDU** | **Frames** |

**🔑 Key Concepts:**
- **MAC Address**: 48-bit hardware identifier (e.g., `00:1A:2B:3C:4D:5E`)
- **Ethernet Frame**: Contains source MAC, destination MAC, type, payload, CRC
- **Switch MAC Table**: Learns which MAC lives on which port to avoid flooding

```
⚠️ COMMON LAYER 2 ISSUES:
   • MAC address table full (CAM table overflow)
   • VLAN misconfiguration
   • Duplex mismatch (half vs full)
   • STP (Spanning Tree) blocking ports
   • ARP poisoning attacks
```

> 💡 **Security Note**: ARP spoofing and MAC flooding attacks happen at Layer 2. This is why port security and dynamic ARP inspection exist!

---

### 🌐 Layer 3: Network Layer

> *"The routing layer — where IP addresses decide the path."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Logical addressing and routing across DIFFERENT networks |
| **Addressing** | **IP addresses** (IPv4 / IPv6) |
| **Key device** | **Routers** — forward packets based on destination IP |
| **PDU** | **Packets** |

**🔑 Key Concepts:**
- **IP Address**: Logical, hierarchical address (can change; not tied to hardware)
- **Subnet Mask**: Defines network vs host portion of IP
- **Default Gateway**: Router's IP on your local network
- **Routing Table**: Map of paths to destination networks

```
⚠️ COMMON LAYER 3 ISSUES:
   • Wrong IP address / subnet mask
   • Default gateway unreachable
   • Routing loop (count to infinity)
   • ICMP blocked by firewall (can't ping)
   • IP conflict (duplicate IP addresses)
```

> 💡 **Exam Tip**: Know the difference between **routed protocols** (IP, IPX) and **routing protocols** (OSPF, EIGRP, BGP, RIP).

---

### 📦 Layer 4: Transport Layer

> *"The post office of networking — ensuring your data arrives intact."*

| Aspect | Details |
|:-------|:--------|
| **Function** | End-to-end delivery, segmentation, reassembly, flow control |
| **Protocols** | **TCP** (reliable) and **UDP** (fast/unreliable) |
| **Addressing** | **Port numbers** (0-65535) |
| **PDU** | **Segments** (TCP) / **Datagrams** (UDP) |

**🔥 TCP vs UDP — The Classic Battle**

| Feature | TCP 🔒 | UDP ⚡ |
|:--------|:-------|:-------|
| **Connection** | Connection-oriented | Connectionless |
| **Reliability** | Guaranteed delivery (acknowledgments) | Best-effort delivery |
| **Order** | Sequenced and ordered | No ordering |
| **Speed** | Slower (overhead) | Faster (minimal overhead) |
| **Use cases** | Web (HTTP/HTTPS), Email, FTP | DNS, VoIP, Streaming, Gaming |
| **Handshake** | 3-way: `SYN → SYN-ACK → ACK` | None |
| **Header size** | 20-60 bytes | 8 bytes |

**🤝 The 3-Way Handshake (TCP Connection Establishment)**

```
     CLIENT                                    SERVER
        │                                        │
        │ ─────────────── SYN ─────────────────> │  "I want to talk"
        │                                        │
        │ <────────── SYN + ACK ──────────────── │  "I got it, you there?"
        │                                        │
        │ ─────────────── ACK ─────────────────> │  "I'm here, let's go!"
        │                                        │
        │◄══════════ CONNECTION ESTABLISHED ═════│
```

**Well-Known Port Numbers to Memorize:**

| Port | Protocol | Service |
|:----:|:---------|:--------|
| 20/21 | FTP | File Transfer |
| 22 | SSH | Secure Shell |
| 23 | Telnet | Remote access (insecure!) |
| 25 | SMTP | Email sending |
| 53 | DNS | Domain Name System |
| 80 | HTTP | Web (unencrypted) |
| 110 | POP3 | Email retrieval |
| 143 | IMAP | Email retrieval |
| 443 | HTTPS | Web (encrypted) |
| 3389 | RDP | Remote Desktop |

---

### 🔗 Layer 5: Session Layer

> *"The connection manager — establishing, maintaining, and tearing down sessions."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Session establishment, maintenance, and termination between applications |
| **Key concepts** | Session management, dialog control, checkpointing |
| **Protocols** | NetBIOS, RPC, PPTP, SOCKS |
| **Tunneling** | Encapsulating data within existing sessions |

**Real-World Example:**
- When you log into a website, the Session Layer manages your login session
- If you lose connection and reconnect, session recovery can resume where you left off

---

### 🔐 Layer 6: Presentation Layer

> *"The translator — making data readable and secure."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Data formatting, encryption/decryption, compression, character encoding |
| **Key concepts** | SSL/TLS encryption, ASCII/UTF-8, JPEG/MPEG, data compression |
| **SSL/TLS** | Historically associated here; modern implementations often at Layer 7 |

**What happens here:**
- Your password is encrypted before being sent over the network
- Images are compressed (JPEG) or encoded (Base64)
- Character sets are translated (EBCDIC → ASCII)

---

### 👤 Layer 7: Application Layer

> *"What the user actually sees and interacts with."*

| Aspect | Details |
|:-------|:--------|
| **Function** | Interface between user applications and network services |
| **NOT the application itself** | Chrome, Outlook, Zoom are apps — HTTP, SMTP, SIP are Layer 7 protocols |
| **Common protocols** | HTTP/HTTPS, FTP, DNS, DHCP, SNMP, POP3, IMAP, SIP |

```
⚠️ COMMON LAYER 7 ISSUES:
   • Wrong URL or DNS resolution failure
   • Application misconfiguration
   • Authentication failures
   • Firewall blocking specific application traffic
   • Malware/viruses operating at application level
```

---

## 🔬 Real-World Mapping: Technologies to Layers

| Layer | Technology/Concept | Why It Fits |
|:-----:|:-------------------|:------------|
| **L1** | Cables, fiber, wireless signals, hubs | Raw physical transmission |
| **L1** | Loopback tests | Verifying hardware signal integrity |
| **L2** | Ethernet frames, MAC addresses | Hardware addressing and local delivery |
| **L2** | Network switches | Forward based on MAC addresses |
| **L3** | IP addresses, subnet masks | Logical addressing across networks |
| **L3** | Routers, routing tables | Forward packets between networks |
| **L4** | TCP ports, UDP ports | End-to-end process delivery |
| **L4** | TCP handshake, windowing | Connection management and flow control |
| **L5** | Session cookies, tunneling | Persistent connection management |
| **L6** | SSL/TLS certificates | Encryption and data formatting |
| **L7** | HTTP requests, DNS queries | User-visible application protocols |

---

## 🦈 Wireshark Deep Dive: Seeing OSI in Action

**Wireshark is your X-ray vision into network traffic.** It breaks down every captured frame to show you exactly which OSI layers are involved.

### Wireshark's Three-Pane View

```
┌─────────────────────────────────────────────────────────────────────┐
│  Pane 1: Packet List                                               │
│  ┌────┬──────────┬─────────────┬──────────────┬─────────────────┐ │
│  │ No │  Time    │  Source     │ Destination  │ Protocol │ Info │ │
│  ├────┼──────────┼─────────────┼──────────────┼─────────────────┤ │
│  │ 88 │ 4.823120 │ 192.168.1.5 │ 142.250.80.3 │  HTTPS   │ GET  │ │
│  └────┴──────────┴─────────────┴──────────────┴─────────────────┘ │
├─────────────────────────────────────────────────────────────────────┤
│  Pane 2: Protocol Tree (Layer Breakdown)                           │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │ ▼ Frame 88: 2005 bytes on wire, 2005 bytes captured           │ │
│  │   → Layer 1: Physical (frame size, interface)                 │ │
│  │   ▼ Layer 2: Ethernet II                                     │ │
│  │       Source MAC: Apple_12:34:56 (00:1a:2b:12:34:56)         │ │
│  │       Destination MAC: Cisco_78:9a:bc (00:1c:2d:78:9a:bc)    │ │
│  │   ▼ Layer 3: Internet Protocol Version 4                     │ │
│  │       Source IP: 192.168.1.5                                  │ │
│  │       Destination IP: 142.250.80.3 (google.com)               │ │
│  │   ▼ Layer 4: Transmission Control Protocol                   │ │
│  │       Source Port: 54321                                      │ │
│  │       Destination Port: 443 (HTTPS)                           │ │
│  │   ▼ Layer 5-7: SSL/TLS + Application Data                   │ │
│  │       TLSv1.3: Application Data (encrypted)                 │ │
│  └────────────────────────────────────────────────────────────────┘ │
├─────────────────────────────────────────────────────────────────────┤
│  Pane 3: Raw Data (Hex + ASCII)                                    │
│  ┌────────────────────────────────────────────────────────────────┐ │
│  │ 0000  00 1c 2d 78 9a bc 00 1a 2b 12 34 56 08 00 45 00  │..│ │
│  │ 0010  07 d5 00 00 40 00 40 06 b2 1e c0 a8 01 05 8e fa  │..│ │
│  │ ... (hexadecimal dump of the entire frame)                     │ │
│  └────────────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────────────┘
```

### How SSL/TLS Wraps Layers 5-7

```
┌────────────────────────────────────────┐
│  In an HTTPS connection:               │
│                                          │
│  Layer 7 (Application)  ──┐            │
│  Layer 6 (Presentation) ──┼── SSL/TLS │
│  Layer 5 (Session)      ──┘            │
│                                          │
│  SSL/TLS encrypts and manages:           │
│  • Session keys (Layer 5)                │
│  • Encryption/decryption (Layer 6)     │
│  • HTTP payload (Layer 7)                │
└────────────────────────────────────────┘
```

> 🔑 **Key Insight**: In modern encrypted traffic, SSL/TLS effectively bundles the top three layers. Wireshark shows this as a single SSL/TLS entry covering Layers 5-7.

---

## 🛠️ Troubleshooting with the OSI Model

### The Layered Diagnostic Approach

When something breaks, work systematically:

```
┌─────────────────────────────────────────────────────────────┐
│  BOTTOM-UP TROUBLESHOOTING (Most Common)                    │
│                                                             │
│  1. Layer 1: Is the cable plugged in? LEDs on?              │
│  2. Layer 2: Is the switch port up? MAC address learned?    │
│  3. Layer 3: Can I ping the gateway? IP config correct?     │
│  4. Layer 4: Is the port open? Firewall blocking?           │
│  5. Layer 7: Is the web server responding? DNS working?     │
│                                                             │
│  TOP-DOWN TROUBLESHOOTING (Application Issues)              │
│                                                             │
│  1. Layer 7: Is the website loading? DNS resolving?         │
│  2. Layer 6: Is SSL certificate valid?                    │
│  3. Layer 5: Is the session timing out?                     │
│  4. Layer 4: Is TCP connection establishing?                │
│  5. Layer 3: Can I reach the server IP?                   │
└─────────────────────────────────────────────────────────────┘
```

### Real Scenario: "I can't access the website"

| Step | Layer | Test | Result | Diagnosis |
|:----:|:-----|:-----|:-------|:----------|
| 1 | L1 | Check cable/LEDs | ✅ Solid green | Not Layer 1 |
| 2 | L2 | Check switch port | ✅ Port up | Not Layer 2 |
| 3 | L3 | `ping 8.8.8.8` | ✅ Replies | Not Layer 3 |
| 4 | L4 | `telnet google.com 443` | ❌ Timeout | **Layer 4!** Firewall blocking 443 |
| 5 | L7 | Browser test | N/A | Would work if L4 passed |

**Result**: Issue is at Layer 4 — firewall or ACL blocking HTTPS port 443.

---

## 💡 Pro Tips & Exam Insights

> 🔑 **For CompTIA Network+ N10-009:**
> - Expect 5-10 questions directly testing OSI layer knowledge
> - Know which **devices** operate at each layer (switch=L2, router=L3, firewall=L3-L7)
> - Know which **protocols** belong to each layer
> - Be ready to troubleshoot using top-down vs bottom-up approaches

> 🔑 **For Security+ SY0-701:**
> - Understand **where attacks happen**: ARP spoofing (L2), IP spoofing (L3), port scanning (L4), SQL injection (L7)
> - Know **where defenses live**: Port security (L2), ACLs (L3), firewalls (L3-L4), WAF (L7)

> 🔑 **For Real-World Pentesting:**
> - Layer 2 attacks: ARP spoofing, MAC flooding, VLAN hopping
> - Layer 3 attacks: IP spoofing, ICMP redirect, route poisoning
> - Layer 4 attacks: SYN flood, UDP flood, port scanning
> - Layer 7 attacks: SQL injection, XSS, buffer overflow

---

## 🧠 Quick Quiz — Test Your Knowledge!

**1. At which layer does a network switch primarily operate?**
<details>
<summary>💡 Click to reveal</summary>

**Layer 2 — Data Link Layer**
Switches forward frames based on destination MAC addresses. (Though Layer 3 switches can also route!)
</details>

**2. What is the PDU called at Layer 4?**
<details>
<summary>💡 Click to reveal</summary>

**Segment** (TCP) or **Datagram** (UDP)
Remember: Layer 2 = Frame, Layer 3 = Packet, Layer 4 = Segment/Datagram
</details>

**3. Which layer handles the TCP 3-way handshake?**
<details>
<summary>💡 Click to reveal</summary>

**Layer 4 — Transport Layer**
SYN → SYN-ACK → ACK happens here, establishing reliable connections.
</details>

**4. A user reports they can't access any websites, but they can ping their router. Which layer is most likely the problem?**
<details>
<summary>💡 Click to reveal</summary>

**Layer 7 — Application Layer** (or possibly Layer 4 if DNS/ports are blocked)
Since Layer 3 (ping to router) works, the issue is higher up. Check DNS resolution, browser settings, or proxy configurations.
</details>

**5. In Wireshark, you see "SSL" in the protocol column. Which OSI layers does this represent?**
<details>
<summary>💡 Click to reveal</summary>

**Layers 5, 6, and 7 combined**
SSL/TLS handles session management (L5), encryption/presentation (L6), and application data (L7) in one encapsulation.
</details>

---

## 📝 Today's Lab Exercise

### Lab 1.1: Map Your Home Network to OSI Layers

**Objective**: Identify which OSI layer each component of your home network operates at.

**Steps**:
1. List every network device you can see (router, switch, laptop, phone, smart TV)
2. For each device, identify the highest OSI layer it operates at
3. Draw a simple diagram showing the flow of data from your laptop to Google.com, labeling each layer

**Example Answer**:
```
[Laptop] ──L1──> [Ethernet Cable] ──L1──> [Router/Switch] ──L2/L3──> 
[ISP Modem] ──L1/L2──> [Internet] ──L3──> [Google Server]
```

### Lab 1.2: Wireshark OSI Analysis

**Objective**: Capture traffic and identify OSI layers in Wireshark.

**Steps**:
1. Download and install Wireshark: https://www.wireshark.org/
2. Start a capture on your active interface
3. Open a browser and visit `http://example.com` (not HTTPS for now)
4. Stop the capture
5. Find the HTTP GET request and expand the protocol tree
6. Identify and screenshot each OSI layer present

**What to document**:
- Frame number: ___
- Layer 2 protocol: ___
- Source MAC: ___
- Layer 3 protocol: ___
- Source/Destination IP: ___
- Layer 4 protocol: ___
- Source/Destination Port: ___
- Layer 7 protocol: ___

---

## 📚 References & Resources

| Resource | Link | Description |
|:---------|:-----|:------------|
| Professor Messer N10-009 | https://www.youtube.com/@professormesser | Free Network+ course |
| Wireshark Download | https://www.wireshark.org/ | Protocol analyzer tool |
| OSI Model Official | https://www.iso.org/standard/20269.html | ISO standard reference |
| TCP/IP Guide | http://www.tcpipguide.com/ | Free online TCP/IP guide |
| Cisco OSI Overview | https://learningnetwork.cisco.com/ | Cisco Learning Network |

---

## 🏷️ Tags

`#Networking` `#OSI-Model` `#CompTIA` `#NetworkPlus` `#N10-009` `#Wireshark` `#TCP-IP` `#Layer1` `#Layer2` `#Layer3` `#Layer4` `#Layer7` `#CyberSecurity` `#90DaysOfCyberSecurity`

---

*📅 **Day 1 of 90** | 🗓️ Date: ___________ | ⏱️ Study Time: ___ hours*

*✍️ Notes by: [Your Name]*
*🎓 Following: 90DaysOfCyberSecurity Study Plan*

---

> 🌟 **Next Up**: [Day 2 — IP Addressing & Subnetting](Day-02-IP-Addressing.md)
