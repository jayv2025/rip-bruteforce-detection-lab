Incident Response Report

December 16, 2025

Executive Summary

A simulated remote brute-force attack was conducted against on Windows 11 Pro (ARM) virtual machine. The attack originated from a separate Kali Linux virtual machine on the same private network hosted on macOS. The activity targeted a standard local user account (testuser) over Remote Desktop Protocol (RDP, TCP port 3389). Although no unauthorized access was achieved, repeated failed authentication attempts were detected through Windows Security Event Logs (Event ID 4625, Logon Type 3). This exercise demonstrated effective host-based detection, log analysis and mapping of adversary behavior to the MITRE ATT&CK framework.

Incident Overview:

**Victim:** Windows 11 Pro (ARM) VM

**Attacker:** Kali Linux VM

**Attack Type:** Remote brute-force RDP authentication attempts

**Target Account:** testuser (standard users)

**Analyst Account:** labadmin (administrator, monitored logs)

**Data Source:** Windows Security Event Logs

**Objective:** Simulate credential brute-force attack and analyze detection/logging

Timeline of Events:

| Local Time | Source | Event | Details |
| --- | --- | --- | --- |
| 13:20 | Analyst Action | Environment preparation | Enable advanced audit policies for logon/logoff (success & failure). Verified RDP service availability on Windows 11 Pro victim VM |
| 13:33 | Attacker -Kali | Attack initiated | Hydra launched against victim IP targeting RDP (TCP 3389) using a password wordlist |
| 13:33-13:34 | Windows Security Logs | Authentication Failures | Multiple Event ID 4625 entries generated for account testuser |
| 13:34 | Windows Security Logs | Logon Type Observed | Logon type 3 (network), indicating repeated remote authentication attempts without interactive session creation |
| 13:34 | Windows Security Logs | Source Attribution | All failed logon originated from attacker IP 192.168.213.128 |
| 13:35 | Analyst Action | Attack Terminated | Hydra manually stopped after sufficient failed authentication evidence was observed |
| 13:36 | Analyst Action | Evidence Collection | Security event logs exported for analysis and documentation |

Evidence and Findings:

**Windows Security Event Logs:**

Event ID 4625 (Failed Logon)

**Target Account**: testuser

**Source IP:** 192.168.213.128

**Pattern:** Repeated rapid authentication failures

**Observations:**

- No successful authentication occurred
- Security logs functioned as intended, detecting attack attempts
- Preventive controls (e.g., account lockout, MFA, RDP restrictions) were not enforced, allowing repeated attacks

MITRE ATT&CK Mapping:

| Technique ID | Technique Name | Description |
| --- | --- | --- |
| T1110 | Brute Force | Repeated attempt to guess credentials against a valid account |
| T1078 | Valid Accounts (Attempted, Unsuccessful) | Targeted use of a legitimate account, even though authentication failed |

Although authentication was unsuccessful, the attack targeted a valid local account, aligning with attempted misuse of valid credentials.

Impact Assessments:

**Access Gained:** None

**Potential Impact If Successful:**

- Unauthorized account access
- Lateral movement
- Privilege escalation
- Persistence / backdoor installation
- Data exfiltration

**Failed/Missing Controls:**

- Preventive controls were not configured, allow repeated authentication attempts without restrictions. (e.g., account lockout, MFA, RDP restrictions)
- Logging/Auditing controls worked as expected

Remediation & Recommendations

- Enable account lockout policies to block repeated failed attempts
- Implement multi-factor authentication for RDP users
- Restrict RDP access by IP or VPN
- Monitor Event ID 4625 and configure alerts for repeated failed logins
- Consider host-based firewall rules to limit attack surface