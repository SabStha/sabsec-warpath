## 🧩 Detection Mapping (MITRE ATT&CK)
1.**Nmap Scan (T1046)**: High-volume connection attempts to multiple ports from one source IP; IDS/IPS or SIEM could flag abnormal scan patterns.
2.**Samba Exploit (T1190):** Signature-based detection of known CVE-2007-2447 payloads (Metasploit modules leave fingerprints).
3.**Reverse Shell (T1059.004):** Outbound connection from unusual user (`nobody`) to attacker IP; could be caught via EDR or NetFlow monitoring.
4.**Privilege Escalation (T1068):** Execution of unknown kernel exploit binary (e.g., `rootme`); flagged by AV or process anomaly tools.
