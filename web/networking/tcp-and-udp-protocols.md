---
description: List of common protocols
---

# TCP & UDP Protocols

| TCP                                            | UDP                                                  | Both                                        |
| ---------------------------------------------- | ---------------------------------------------------- | ------------------------------------------- |
| FTP (File Transfer Protocol) - 20, 21          | SNMP (Simple Network Management Protocol) - 161, 162 | LDAP (Lightweight Directory Protocol) - 389 |
| TFTP (Trivial File Transfer Protocol) - 69     | NTP (Network Time Protocol) - 123                    | RDP (Remote Desktop Protocol) - 3389        |
| SFTP (Secure File Transfer Protocol) - 22      | SIP (Session Initiation Protocol) - 5060, 5061       | DNS (Domain Name System) - 53               |
| SSH (Secure Shell) - 22                        | RTSP (Real-Time Streaming Protocol) - 554            |                                             |
| TELNET                                         | DHCP (Dynamic Host Configuration Protocol) - 67, 68  |                                             |
| SMTP (Simple Mail Transfer Protocol) - 25, 465 |                                                      |                                             |
| IMAP 4 - 143, 993                              |                                                      |                                             |
| POP 3 - 110, 995                               |                                                      |                                             |
| HTTP - 80                                      |                                                      |                                             |
| HTTPS - 443                                    |                                                      |                                             |

### Details of each Protocols



#### Discovery Related Protocol

* SNMP (UDP)
  * Simple Network Management Protocol
  * It collects information in your network about routers, switches, etc
* NTP (UDP)
  * Network Time Protocol
  * Port - 123
  * Used for Time Synchronization
* DHCP (UDP)
  * Dynamic Host Configuration Protocol
  * Port - 67, 68
  * Provides an IP address to computers, generally&#x20;
  * The computers use DORA to fetch IP Address for your
    * Provides IP Address
    * Subnet mask
    * Default gateway
    * DNS
* LDAP (Both)
  * Lightweight Directory Access Protocol
  * Port - 389
  * Active Directory

#### For Web (All TCP)

* HTTP
  * HyperText Transfer Protocol
  * Port - 80
* HTTPS
  * HyperText Transfer Protocol Secure
  * Port - 443
  * Uses TLS/SSL

#### For Videos and Streaming (All UDP)

* SIP
  * Session Initiation Protocol
  * Port- 5060, 5061
    * 5060 - transferred as clear text
    * 5061 - using TLS (secure)
  * For video and voice between two&#x20;
* RTSP
  * Real-Time Streaming Protocol
  * Port - 554
  * For streaming videos

#### File Transfer Protocols (All TCP)

* FTP
  * File Transfer Protocol
  * Port
    * 20 - used to transfer the data
    * 21 - just establish the connection and maintain it
  * Ask for username/password
* TFTP
  * Trivial File Transfer Protocol
  * Port - 69
  * Doesn't ask for username/password
* SFTP
  * Secure File Transfer Protocol
  * Port - 22
  * Encrypted during transfer
  * Uses SSH

**Remote Connection Protocol**

* SSH (TCP)
  * Secure Shell
  * Port - 22
* TELNET (TCP Only)
  * Port - 23
  * Not secure.
  * Only CLI support
* RDP (Both)
  * Remote Desktop Protocol
  * Port - 3389
  * Connect to another computer and manage it (Remote Desktop Connection)

#### Email Related Protocol (All TCP)

* SMTP
  * Simple Mail Transfer Protocol
  * Port
    * 25 - clear text
    * 465 - TLS/SSL
  * Communication between two Mail servers
* IMAP 4
  * Internet Message Access Protocol
  * Port -&#x20;
    * 143 - clear text
    * 993 - TLS/SSL
  * The Client receives a copy of data
* POP 3
  * Post Office Protocol
  * Port -
    * 110 - clear text
    * 995 - TLS/SSL
  * The client receives original mail, and once deleted from the client, it's gone forever
