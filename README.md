🟦 Incident Response Simulation
1. Objective

Simulate and respond to a cybersecurity incident within a controlled environment to demonstrate detection, containment, eradication, and recovery processes. Focus on identifying attack vectors, understanding impact, and applying mitigation strategies.

2. Environment
   
Virtual machines (VMware) simulating a corporate network

Operating systems: Windows Server, Windows 11 clients.

Security tools: WireShark and Wazuh.

3. Attack Simulation

A simulated malicious scenario was created where a user downloads executable files from an internal web server.

A Microsoft IIS instance was configured on Windows Server to host multiple executable files in the default directory:

update_kb501.exe
chrome_patch.exe
malware-fake.exe

From a Windows 11 machine, the attacker simulated suspicious activity using:

Web browser access
PowerShell command execution (Invoke-WebRequest)

This behavior mimics a compromised host downloading potentially malicious payloads.

4. Data Collection

Network traffic was captured using Wireshark on a Ubuntu machine.

A capture filter was not applied, but analysis focused on HTTP traffic using:

http

Relevant packets included:

HTTP GET requests
Server responses (HTTP 200 OK)
File transfer activity

5. Analysis

The captured traffic revealed multiple HTTP requests for executable files.

Key observations:

Repeated GET requests for .exe files
All requests originated from the same source IP
Short time interval between downloads
Use of both browser and PowerShell (different User-Agents)
<img width="1275" height="769" alt="image" src="https://github.com/user-attachments/assets/33ee5bbf-5455-422b-b5d3-4b5de2959ef4" />

Example pattern observed:

GET /chrome_patch.exe

<img width="1916" height="836" alt="image" src="https://github.com/user-attachments/assets/8d6b1f87-8fcf-4f8d-a4ca-544310853e61" />

GET /update_kb5012.exe

<img width="1684" height="840" alt="image" src="https://github.com/user-attachments/assets/6814f15c-370d-46a8-aeaa-9ac00a1bf005" />

GET /malware-fake.exe

<img width="1591" height="792" alt="image" src="https://github.com/user-attachments/assets/dd6040b9-aeab-4e26-a010-a85278839d7d" />

This pattern is commonly associated with:

Malware staging
Initial payload delivery
User-driven execution after phishing

6. Findings

The analysis identified suspicious behavior consistent with potential malware activity:

Multiple executable downloads over HTTP
Lack of encryption (clear-text traffic)
Repetitive access pattern from a single host
Indicators of automated or scripted activity

Additionally:

HTTP allows full visibility of transferred files
No authentication or validation mechanism was observed

7. Mitigation

To reduce the risk of similar threats:

Enforce HTTPS instead of HTTP
Implement web proxy filtering
Block or inspect .exe downloads
Deploy Endpoint Detection and Response (EDR) solutions
Monitor PowerShell activity (e.g., Invoke-WebRequest)
Apply network-based detection rules (IDS/IPS)

8. Conclusion

This lab demonstrates how unencrypted HTTP traffic can expose suspicious file download activity.

By analyzing network packets, it is possible to identify:

Malicious patterns
Indicators of compromise
User or system behavior anomalies

Combining network monitoring with endpoint logging (e.g., via Wazuh) significantly improves detection capabilities in real-world environments.

9. Incident Response

Upon identifying suspicious HTTP downloads, the following response actions were considered:

Containment
Isolated the affected host from the network
Blocked communication with the web server hosting the files

Eradication
Removed downloaded executable files from the system
Scanned the host for malware using security tools

Investigation
Reviewed additional logs (PowerShell, system events)
Correlated activity with SIEM alerts using Wazuh

Recovery
Restored the system to a trusted state
Re-enabled network access after validation

Lessons Learned
HTTP traffic allowed file inspection but lacks security
Monitoring PowerShell usage is critical
User awareness is essential to prevent similar incidents.
