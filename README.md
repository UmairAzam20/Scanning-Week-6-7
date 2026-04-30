# Scanning-Week-6-7
This document provides a structured security assessment across network packet analysis, FTP traffic decoding, service enumeration, OS fingerprinting, and vulnerability scanning.

## 1. Question 1: Packet Analysis – ICMP & Encoding

**Objective**

Extract the hidden flag from packet1.pcap using packet capture analysis and Base64 decoding.

**Methodology**
1. Inspect ICMP traffic in Wireshark.
2. Confirm host reachability and TTL behavior.
3. Extract the encoded payload, then decode it.

**Findings**
1. ICMP echo requests and replies were successfully observed.
2. TTL values confirmed the target host responded correctly.
3. An encoded payload string was recovered from the packet.
   
**Result**
Encoded string: U1VCVEVYDYt2e2FpX21zX2Nvb30=
Decoded flag: SUCTF2023{ai_is_cool}
