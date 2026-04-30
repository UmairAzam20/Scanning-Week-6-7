# Scanning-Week-6-7
This document provides a structured security assessment across network packet analysis, FTP traffic decoding, service enumeration, OS fingerprinting, and vulnerability scanning.

## 1. Question 1: Packet Analysis – ICMP & Encoding

**Objective**

Extract the hidden flag from packet1.pcap using packet capture analysis and Base64 decoding.

**Methodology**
* Inspect ICMP traffic in Wireshark.
* Confirm host reachability and TTL behavior.
* Extract the encoded payload, then decode it.

**Findings**
* ICMP echo requests and replies were successfully observed.
* TTL values confirmed the target host responded correctly.
* An encoded payload string was recovered from the packet.
   
**Result**

Encoded string: U1VCVEVYDYt2e2FpX21zX2Nvb30=

Decoded flag: SUCTF2023{ai_is_cool}

*WireShark ICMP Finding*

![Nmap Scan Results](screenshot/packet1.png)

*Base64 decoded*

![Nmap Scan Results](screenshot/packet1%20flag.png)


## 2. Question 2: FTP Traffic & Cipher Decoding

**Objective**

Analyze packet2.pcap to recover the final flag using FTP data extraction and multi-stage decoding.

**Methodology**

* Confirm host availability via ICMP.
* Capture FTP traffic and export relevant files.
* Expand shortened URLs or clues.
* Decode the identified cipher system.
* Perform final Base64 decoding.

**Decoding Process**

* Confirmed FTP host communication and extracted evidence.
* Expanded the TinyURL clue to reveal the hidden text.
* Decoded the Pigpen / Tic-Tac-Toe cipher.
* Applied Base64 decoding to obtain the final flag.

  
**Result**

* Pigpen output: EXMACHINAAVA
* Final flag: SUCTF2023{EXMACHINAAVA}


*WireShark FTP Finding*

![Nmap Scan Results](screenshot/pcket2.png)


*FTP Evindence*

![Nmap Scan Results](screenshot/pcket2step2.png)

*URL Evindence*

![Nmap Scan Results](screenshot/pcket2step3.png)

*PigPen Cipher Decode: Tic Tac Toe*

![Nmap Scan Results](screenshot/CODEEE.png)


## Question 3: Nmap Output Interpretation

**Objective**

Interpret Nmap scan results to identify exposed services, risk levels, and potential attack vectors.

**Nmap Scan** 

| PORT     | STATE | SERVICE      | VERSION                                    |
|----------|-------|--------------|--------------------------------------------|
| 21/tcp   | open  | 6p           | vs6pd 2.3.4                                |
| 22/tcp   | open  | ssh          | OpenSSH 5.3p1                              |
| 80/tcp   | open  | hBp          | Apache 2.2.8                               |
| 139/tcp  | open  | netbios-ssn  |                                            |
| 445/tcp  | open  | microso6-ds  | Windows 7 Professional 7601 Service Pack 1 |


**Risk Assessment**

| Port          | Service                | Attacker Capability                                                                                                     |
|---------------|------------------------|--------------------------------------------------------------------------------------------------------------------------|
| 21 (FTP)      | vsftpd 2.3.4           | Upload/download files, brute force credentials, exploit known vulnerabilities (backdoor, buffer overflow)               |
| 22 (SSH)      | OpenSSH 5.3p1          | Brute force login, if credentials gained → remote shell access                                                          |
| 80 (HTTP)     | Apache 2.2.8           | Web application attacks (SQLi, XSS, LFI), directory traversal, known Apache exploits                                    |
| 139/445 (SMB) | Windows 7 SP1          | Enumerate shares, brute force SMB credentials, exploit EternalBlue (MS17‑010), pass the hash, remote code execution     |


**Known Vulnerabilities**

| Service & Version       | Known Vulnerabilities (CVEs)                                                                                             |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------|
| vsftpd 2.3.4            | Backdoor command (CVE-2011-2523) – port 6200 opens with root shell; also smurf attack, denial of service                 |
| OpenSSH 5.3p1           | Older version – vulnerable to CVE-2016-6210 (user enumeration), CVE-2008-5161 (cipher downgrade), possibly CVE-2014-2532 (bypass) |
| Apache 2.2.8            | Many vulnerabilities: CVE-2017-3167 (auth bypass), CVE-2011-3368 (mod_proxy reverse proxy), CVE-2009-3555 (TLS renegotiation), etc. |
| Windows 7 SP1 (SMB)     | Highly vulnerable to EternalBlue (CVE-2017-0144) – unauthenticated remote code execution, CVE-2017-0143 to CVE-2017-0148, also MS08-067 if older |


## 4. Question 4: OS Fingerprinting via TTL

**Objective**

Identify the operating systems of hosts based on TTL values in captured packets.

**TTL Fingerprinting**

* TTL = 64 → likely Linux / Unix-based host.
  
![Nmap Scan Results](screenshot/CODEEE.png)

* TTL = 128 → likely Windows host.

![Nmap Scan Results](screenshot/CODEEE.png)


* TTL = 255 → likely router or network appliance.

![Nmap Scan Results](screenshot/CODEEE.png)
  
**Conclusion**

TTL fingerprinting confirms the OS families present and supports the vulnerability assessment.
