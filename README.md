Wazuh Windows Detection Lab
Objective
To simulate real-world attacker behaviours on a Windows endpoint and validate detection capabilities using Wazuh SIEM, focusing on authentication, privilege escalation, and defence evasion.

Lab Setup
Wazuh Manager: Ubuntu
Endpoint: Windows 10
Log Source: Windows Security Event Logs
Integration: Wazuh agent configured for real-time log forwarding

Scenarios Simulated
1. Failed Logon Attempts (Event ID 4625)
Simulated multiple failed login attempts by entering incorrect passwords.
Purpose:
 To replicate brute-force or password guessing attacks.
Result:
 Wazuh successfully ingested and detected failed authentication attempts.

2. Successful Logon After Failures (4624 + 4625)
Performed multiple failed logins followed by a successful login.
Purpose:
 To simulate a potential account compromise after brute-force attempts.
Result:
 Observed both failed and successful logon events, forming the basis for correlation-based detection.

3. Privilege Escalation (Event ID 4732)
Added a user to the local Administrators group.
Command used:
net localgroup Administrators testuser /add
Purpose:
 To simulate an attacker gaining elevated privileges.
Result:
 Wazuh detected group membership changes, confirming privilege escalation visibility.

4. Audit Log Clearing (Event ID 1102)
Cleared Windows Security logs.
Command used:
wevtutil cl Security
Purpose:
 To simulate attacker defence evasion by deleting logs.
Result:
 Wazuh generated alerts for log clearing, indicating high-risk activity.

Detection Engineering
Custom Rule 1: Brute Force Detection
Detects multiple failed login attempts within a short timeframe.
<rule id="100001" level="10" frequency="5" timeframe="60">
 <if_matched_sid>60122</if_matched_sid>
 <description>Multiple failed Windows logons detected - possible brute force attack.</description>
</rule>
What it proves:
 Ability to move from single-event detection to behaviour-based detection.

Custom Rule 2: Successful User Login Detection
Filters real user logins to avoid noise from system events.
<rule id="100002" level="12">
 <if_sid>60106</if_sid>
 <field name="win.eventdata.targetUserName">ABHI</field>
 <description>Successful user login detected</description>
</rule>
What it proves:
 Ability to refine detections and reduce false positives.

Key Findings
Windows generates high volumes of authentication events, requiring filtering for meaningful detection
Failed logins alone create noise; correlation improves detection quality
Privilege escalation and log clearing are high-confidence attacker behaviours
Wazuh rules can be customised to detect patterns rather than individual events

What Worked Well
Successful ingestion of Windows Security logs into Wazuh
Accurate detection of all simulated attack scenarios
Custom rule creation for brute-force behaviour
Ability to validate detections using real event data

Limitations
No full correlation rule implemented yet (4625 → 4624)
Testing limited to a single endpoint
No integration with external threat intelligence or EDR

Future Improvements
Build correlation rule for failed logins followed by success (compromise detection)
Expand detection to include PowerShell and process execution events
Integrate MITRE ATT&CK mapping more systematically
Test across multiple endpoints for scalability

MITRE ATT&CK Mapping (High-Level)
T1110 – Brute Force → Failed logon attempts
T1078 – Valid Accounts → Successful login
T1098 – Account Manipulation → Privilege escalation
T1070.001 – Clear Windows Event Logs → Log clearing

Conclusion
This lab demonstrates practical SOC-level skills including log analysis, attack simulation, and detection engineering using Wazuh. It highlights the transition from basic event monitoring to behaviour-based detection, which is critical for real-world security operations.

## Project Report

View the full project report (PDF)(./docs/Wazuh-SOC-Analyst-Report.pdf)
