# Wireshark Network Traffic Analysis & Threat Inspection

## Overview
Hands-on network analysis investigating TCP lifecycle negotiation, unencrypted protocol data exposure, HTTP object carving, and reconnaissance detection using Wireshark and Python.

## Environment & Tools
- *Operating System:* Kali Linux
- *Capture Tool:* Wireshark v4.x
- *Test Server:* Custom Python HTTP server (lab.py) running on 127.0.0.1:8081
- *Reconnaissance Tool:* wireshark

## Core Exercises & Findings

### 1. TCP Three-Way Handshake
- Filtered traffic with tcp.stream eq 0 to observe standard RFC 793 connection negotiation:
  - SYN (Client -> Server)
  - SYN, ACK (Server -> Client)

  - ACK (Client -> Server)
  - <img width="1280" height="960" alt="handshake" src="https://github.com/user-attachments/assets/bb3e4674-1910-43ab-bb63-04a7999b324f" />

### 2. HTTP Credential Interception
- Filter: http.request.method == "POST"
- Extract…
- Extracted unencrypted credentials (username=student&password=LabPass123) from the HTTP form URL-encoded body, illustrating cleartext protocol vulnerabilities.

  <img width="1280" height="720" alt="http_stream" src="https://github.com/user-attachments/assets/b8486442-6f8e-4cbb-845b-60357a0fe507" />


### 3. HTTP Object Carving & Integrity Check
- Carved dynamic PNG payload via Wireshark HTTP Object Export.
- Verified file integrity via SHA-256:
  - SHA256(original_logo.png) == SHA256(exported_logo.png)

  - <img width="1280" height="960" alt="port_scan" src="https://github.com/user-attachments/assets/712524e6-ea94-4662-9d3c-ac1bde3bf3a5" />


### 4. TCP Port Scan Fingerprint
- Filter: tcp.flags.syn == 1 && tcp.flags.ack == 0

- Identified scanning signature: Rapid sequential SYN bursts targeting ports 8070–8090 from a single endpoint. Inactive ports return RST, ACK, while open port 8081 returns SYN, ACK.
- <img width="1280" height="960" alt="syn scan" src="https://github.com/user-attachments/assets/0c765779-9714-4ef3-b1a7-7ce884a79eee" />
