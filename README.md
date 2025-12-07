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



🧠 Event Simulation and Collection

Simulated Attack Behaviors



To replicate real-world attacker activity, multiple user and system actions were simulated on the Windows endpoint:



Multiple failed login attempts (Event ID 4625)



Successful logins (Event ID 4624)



New user creation (Event ID 4720)



User account deletion (Event ID 4726)



Privilege escalation (Event ID 4732 – user added to admin group)



Audit log clearing (Event ID 1102)



These actions mimic typical brute-force, privilege escalation, and defense evasion stages in the ATT\&CK chain.



Log Verification



Monitored incoming events on Wazuh via:



/var/ossec/logs/alerts/alerts.log

/var/ossec/logs/ossec.log





Confirmed receipt of Windows security events under Wazuh’s eventchannel source.



Verified rule matching and event parsing via the Wazuh Dashboard.



🧩 MITRE ATT\&CK Mapping

Event ID	Action Simulated	MITRE Technique	Tactic

4625	Failed logon	T1110 – Brute Force	Credential Access

4624	Successful logon	T1078 – Valid Accounts	Initial Access

4720	Account creation	T1136 – Create Account	Persistence

4726	Account deletion	T1531 – Account Deletion	Defense Evasion

4732	Privilege escalation	T1078 / T1069 – Privileged Group Modification	Privilege Escalation

1102	Audit log cleared	T1070.001 – Clear Windows Event Logs	Defense Evasion

🧾 Results



✅ Successfully established real-time event forwarding between Windows and Wazuh Manager.



✅ Captured and analyzed over 50+ security event logs tied to authentication and user management activity.



✅ Mapped detections to MITRE ATT\&CK techniques, aligning with blue-team defensive monitoring practices.



✅ Built a reproducible mini SOC lab foundation for endpoint detection and log analysis.



🛠️ Tools and Technologies



Wazuh Manager 4.9.2 (Ubuntu)



Wazuh Agent (Windows)



Windows Event Viewer



Syslog \& Auth Logs



MITRE ATT\&CK Framework



📈 Outcome



This setup provided a functional, testable SOC simulation that demonstrates the end-to-end flow of:



Endpoint event generation



Log forwarding



Rule-based detection and analysis

