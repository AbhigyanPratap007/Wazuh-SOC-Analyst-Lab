Wazuh SIEM Endpoint Integration and Event Analysis

📘 Project Overview



This project documents the process of deploying a Wazuh SIEM on Ubuntu, connecting a Windows endpoint agent, and analyzing security event logs to simulate a small-scale SOC (Security Operations Center) environment.



The main objective was to collect and analyze real-time Windows security events to detect attacker behaviors and validate detections using MITRE ATT\&CK mapping.



🧩 Environment Setup

1\. Wazuh Manager Installation (Ubuntu)



Installed Wazuh Manager 4.9.2 on an Ubuntu VM.



Verified service health using:



sudo systemctl status wazuh-manager





Ensured logs were generated in:



/var/ossec/logs/ossec.log

/var/ossec/logs/alerts/alerts.log





Configured /var/ossec/etc/ossec.conf to collect local system and authentication logs:



<localfile>

&nbsp; <log\_format>syslog</log\_format>

&nbsp; <location>/var/log/syslog</location>

</localfile>



<localfile>

&nbsp; <log\_format>syslog</log\_format>

&nbsp; <location>/var/log/auth.log</location>

</localfile>



2\. Windows Agent Deployment



Installed and registered the Wazuh Agent on a Windows 10 endpoint.



Connected it to the Ubuntu Wazuh Manager using the manager’s IP and authentication key.



Verified communication through:



sudo /var/ossec/bin/list\_agents -c





Confirmed active event forwarding from Windows to Wazuh Manager.



## 🧩 Key Objectives
- Deploy Wazuh manager and agent to simulate a functional SOC environment  
- Collect Windows Security Logs (Event IDs: 4624, 4625, 4726, 4732, etc.)  
- Simulate attacker actions such as:
  - Failed logins and brute-force attempts  
  - Account deletions and privilege escalations  
  - Audit log clearing activities  
- Map all detections to **MITRE ATT&CK** techniques  

---

## 🔍 Detection Scenarios
- **Brute Force (4625)** — Multiple failed logons detected  
- **Successful Logon (4624)** — Followed by privilege escalation activity  
- **Account Deletion (4726)** — Potential insider threat simulation  
- **Audit Log Clearing (1102)** — Indicator of log tampering  

Each detection was analyzed through Wazuh dashboards, alert logs, and rule correlation.

---

## 📊 Results
- **10+ attacker behaviors** successfully simulated  
- **50+ Windows event logs** analyzed  
- **MITRE Mapping Examples**:
  - T1110 — Brute Force  
  - T1078 — Valid Accounts  
  - T1070 — Indicator Removal on Host  
