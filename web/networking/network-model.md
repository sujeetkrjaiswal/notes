---
description: A model designed to standardise Compute Networking
---

# Network Model

### Layers / Networking Model&#x20;

#### OSI Layers

* Application (Application layer in TCP/IP
* Presentation (Application layer in TCP/IP)
* Session (Application layer in TCP/IP)
* Transport&#x20;
* Network
* Data Link
* Physical

#### Original TCP/IP Model

* Application
* Transport
* Internet
* Link

#### Modified TCP/IP Layer

* Application
* Transport
* Network (Renamed from the Internet)
* Data Link (Split from Link)
* Physical (Split from Link)

### Comparison of Different Models

| OSI Model    | TCP IP Modified | TCP IP Original |
| ------------ | --------------- | --------------- |
| Application  | Application     | Application     |
| Presentation | -               | -               |
| Session      | -               | -               |
| Transport    | Transport       | Transport       |
| Network      | Network         | Internet        |
| Data Link    | Data Link       | Link            |
| Physical     | Physical        | -               |

### Protocols at different layers

| Layers      | Protocols                            |
| ----------- | ------------------------------------ |
| Application | HTTP, FTP, SMTP                      |
| Transport   | TCP, UDP                             |
| Network     | IP, Routers                          |
| Data Link   | Ethernet, Switches                   |
| Physical    | Cables, NIC (Network Interface Card) |

#### Data State at Different Layer

| Layers      | Data State | Content                               |
| ----------- | ---------- | ------------------------------------- |
| Application | Data       | **Data**                              |
| Transport   | Segment    | **TCP-**Data                          |
| Network     | Packet     | **IP-**TCP-Data                       |
| Data Link   | Frame      | **Ethernet-**IP-TCP-Data**-Ethernet** |
| Physical    |            |                                       |

* Data => the actual data to be used (byte stream, images, etc)
* TCP => Port Number
* IP => Source and Destination IP Addresses
* Ethernet => Source and next destination Mac Address

**Why does Data-Link add both header and trailer?**

The Header stores information about source & destination. Whereas the trailer contains some data to check for error by the receiving end.

#### DNS Resolution

Process of converting a Domain name to specific IP Addresses.&#x20;

#### Address Resolution Protocol (ARP)

Process of getting the mac addresses when the IP address of the destination is known

ARP Cache: static & dynamically learned entries

```
arp -a
```

