
Case Study: Ursnif/IcedID Malware Infection Analysis (PCAP Analysis)
📌 Incident Overview
Objective: Identify the initial infection vector, C2 infrastructure, and confirm data exfiltration.
Tool: Wireshark
Source File: 2019-12-19-Ursnif-with-IcedID-and-Valak.pcap
Status: Confirmed Incident (Data Breach)

🕵️‍♂️ Investigation Timeline
Step 1: Identifying the Victim (Internal Recon)
Action: Analyzed network conversations to find the most active internal host.

Method: Statistics -> Conversations -> IPv4 (Sorted by Bytes).
Finding: Internal host 10.12.19.101 showed an anomalous volume of outbound traffic.
📸 Screenshot 1: Conversations table showing 10.12.19.101 as the top talker.
Step 2: Initial Access & DNS Reconnaissance (DGA/C2)
Action: Filtered DNS queries for the victim host to find the entry point.

Filter: ip.src == 10.12.19.101 && dns
Finding:
Initial Vector: A DNS query to the malicious domain impedignaw.com.
DNS Resolution: The response (Packet #46) mapped the domain to Attacker IP 91.90.180.38.
Beaconing: Multiple queries to DGA (Domain Generation Algorithm) domains: zyu33.com, cowspidzu.pro, and shiriez48.com.

📸 Screenshot 2: DNS query/response list showing impedignaw.com and its resolved IP.
Step 3: Malware Delivery & Confirmation
Action: Inspected HTTP traffic with the attacker's IP to verify the payload delivery.

Filter: ip.addr == 91.90.180.38 && http
Findings:
Malicious Payload: An HTTP GET request for /koorsh/soogar.php?l=fakinx9.cab.
Successful Delivery: The server responded with 200 OK, confirming the malware was successfully downloaded to the host.
User-Agent: Mozilla/5.0... suggests the download was triggered via a web browser (likely phishing).

📸 Screenshot 3: HTTP GET request followed by the 200 OK status code.
Step 4: Data Exfiltration (C2 Communication)
Action: Searched for outbound POST requests containing stolen information.

Filter: ip.src == 10.12.19.101 && http.request.method == "POST"
Finding: Multiple POST requests to C2 server 185.22.153.208.
Evidence: The URL contained Base64 encoded strings and tags like NETWORK_INFO, confirming that system data was exfiltrated.
📸 Screenshot 4: HTTP POST request showing the long obfuscated URL used for exfiltration.

🏆 Summary of Findings
The incident is confirmed. Host 10.12.19.101 was compromised by the Ursnif banking trojan.

Infection Vector: Phishing/Web browsing.
Impact: Successful data exfiltration to a Command & Control (C2) server.
Recommendations:

Isolate host 10.12.19.101 immediately.
Block IPs 91.90.180.38 and 185.22.153.208 on the corporate firewall.
Reset all user credentials associated with the affected machine.

💡 SOC Skills Demonstrated:

Network Traffic Analysis (Wireshark)
Threat Hunting (DNS/HTTP)
Identifying Malicious Patterns (DGA, Beaconing, C2)
Incident Reporting

