Network Security Monitoring and Intrusion Detection
Using Nmap, Wireshark, Suricata, and Wazuh
1. Project Overview
This project demonstrates a practical network security monitoring and intrusion detection environment built using VMware Workstation and open-source cybersecurity tools.
The laboratory combines network scanning, packet analysis, intrusion detection, and centralized security event monitoring to investigate suspicious network activity in a controlled virtual environment.
2. Project Objectives
Perform network scanning using Nmap.
Capture and analyze network traffic using Wireshark.
Examine TCP, DNS, and HTTP protocols.
Detect network activity using Suricata IDS.
Forward Suricata events to Wazuh.
Investigate and correlate security events using Wazuh Archives.
3. Lab Environment and Architecture
Component	Role	IP Address
Kali Linux	Nmap, Wireshark, Suricata, Wazuh Agent	192.168.174.131
Metasploitable2	Controlled scanning target	192.168.174.133
Ubuntu Server	Wazuh Manager, Indexer, Dashboard	192.168.174.135
4. Tools and Technologies
Tool	Version	Purpose
VMware Workstation	17.6.2	Virtualization
Ubuntu Server	24.04.5 LTS	Wazuh server
Wazuh	4.14.10	SIEM and security monitoring
Suricata	8.0.6	Network intrusion detection
Wireshark	4.6.6	Network packet analysis
Nmap	7.99	Network scanning
Kali Linux	—	Security testing and monitoring
5. Practical Activities
5.1 Network Traffic Analysis
Wireshark was used to capture and inspect ICMP connectivity traffic, DNS queries and responses, HTTP requests and responses, the TCP three-way handshake, and TCP SYN, SYN-ACK, ACK, and RST packets.
5.2 Network Scanning with Nmap
Service and version detection:
```bash
nmap -sV -T3 --top-ports 100 192.168.174.133
```
TCP SYN scan:
```bash
sudo nmap -sS -T3 -p 1-100 192.168.174.133
```
The scan results were analyzed using Wireshark to identify connection attempts, open ports, and closed-port responses.
5.3 Suricata Intrusion Detection
Suricata was installed and configured on Kali Linux to monitor network traffic through the `eth0` interface. It generated structured security events in `/var/log/suricata/eve.json`.
5.4 Wazuh Integration
The Wazuh Agent on Kali collected Suricata JSON events and forwarded them to the Ubuntu Wazuh Manager.
`Suricata eve.json → Wazuh Agent → Wazuh Manager → Wazuh Archives`
The integration was verified by locating Suricata alert events in Wazuh Archives.
6. Key Investigation Findings
During controlled Nmap testing, Suricata detected HTTP requests associated with the Nmap Scripting Engine.
Detection signature: `ET SCAN Possible Nmap User-Agent Observed`
Field	Finding
Suricata Signature ID	2024364
Suricata Severity	1 — Informational
Wazuh Rule ID	86601
Wazuh Rule Level	3
Source IP	192.168.174.131
Destination IP	192.168.174.133
Destination Port	TCP 80
HTTP Response	404 Not Found
Suricata Action	allowed
The matching events were successfully located in Wazuh Archives. This demonstrated that Suricata detected the Nmap-related HTTP activity and that the Wazuh Agent forwarded the events to the central monitoring server.
The `allowed` action indicates that Suricata detected the traffic without blocking it. The detection does not prove successful exploitation or compromise.
7. Repository Structure
```text
Project-02-Wireshark-Network-Analysis/
├── README.md
├── Documentation/
├── Screenshots/
├── PCAP-Files/
│   ├── nmap-port-scan.pcapng
│   └── tcp-syn-scan.pcapng
├── Investigations/
├── Configuration/
└── Architecture/
```
8. Limitations
Suricata reported significant kernel packet drops during capture. Therefore, the collected traffic may not represent every packet transmitted during the exercises.
The laboratory was conducted in a controlled virtual environment. Detection alerts indicate traffic matching configured rules and should be investigated in context.
9. Learning Outcomes
Network protocol analysis.
Network reconnaissance detection.
Nmap scanning and service identification.
Intrusion detection using Suricata.
SIEM integration and event collection.
Security alert investigation.
Network security monitoring workflows.
10. Conclusion
This project demonstrated how Nmap, Wireshark, Suricata, and Wazuh can work together to support network security monitoring and intrusion detection through packet inspection, detection of Nmap-related activity, and centralized collection of Suricata events.
11. Disclaimer
This project was conducted for educational purposes in a controlled virtual laboratory. All scanning activities were performed against intentionally vulnerable lab systems under controlled conditions.
Metasploitable2 is intentionally vulnerable and should not be exposed to public or untrusted networks.
12. References
Nmap Documentation: https://nmap.org/book/man.html
Wireshark Documentation: https://www.wireshark.org/docs/
Suricata Documentation: https://docs.suricata.io/
Wazuh Documentation: https://documentation.wazuh.com/
VMware Workstation: https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
---
Cybersecurity Portfolio Project — 2026
