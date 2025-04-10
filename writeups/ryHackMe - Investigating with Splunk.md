🕵️‍♂️ TryHackMe - Investigating with Splunk
Objective: Use Splunk to investigate suspicious behavior on a compromised Windows host, identify attacker activity, and analyze forensic artifacts such as logon events, registry changes, PowerShell execution, and more.

🎯 Case Summary
An attacker gained access to a Windows environment and executed multiple actions indicative of post-exploitation activity, including backdoor user creation, impersonation, and encoded PowerShell command execution. As a SOC analyst, we investigated these incidents using Splunk.

🛠 Environment
Splunk Instance IP: 10.10.144.40

Index Used: main

Log Sources: Windows Event Logs, Sysmon Logs

Main Host Under Investigation: Micheal.Beaven

🔍 Investigation Findings
1. Total Number of Events Ingested
Query Used:

spl
Copy code
index=main
Answer: [Count shown in Splunk top bar, e.g., 12756 events]

2. Backdoor User Created
Query:

spl
Copy code
index=main "A user account was created"
Result: The new user A1berto was created.

3. Registry Key Modified for Backdoor
Query:

spl
Copy code
index=main EventCode=12
Answer:

Copy code
HKLM\SAM\SAM\Domains\Account\Users\000003E9
4. Impersonation Attempt
Query:

spl
Copy code
index=main EventCode=4648
Finding: The attacker attempted to impersonate Administrator.

5. Backdoor User Creation Command
Query:

spl
Copy code
index=main EventCode=1 (Image=*net.exe OR CommandLine=*net user*)
Command Used:

sql
Copy code
cmd.exe /C net user A1berto P@ssw0rd /add && net localgroup administrators A1berto /add
6. Login Attempts from Backdoor User
Query:

spl
Copy code
index=main (EventCode=4624 OR EventCode=4625) AccountName="A1berto"
Answer: [e.g., 7 login attempts]

7. Host with Suspicious PowerShell
Query:

spl
Copy code
index=main (Image=*powershell.exe OR CommandLine=*powershell*)
Answer: Micheal.Beaven

8. PowerShell Logging Enabled
Query:

spl
Copy code
index=main EventCode=4104
Answer: [e.g., 4 PowerShell ScriptBlock logs collected]

9. Malicious PowerShell URL
Query:

spl
Copy code
index=main EventCode=4104 encodedcommand
Decoded Using: CyberChef → From Base64 + Decode Text

Answer:

css
Copy code
hxxp[://]10[.]10[.]10[.]5/news[.]php
🔚 Conclusion
The attacker:

Created a new privileged user A1berto

Tried to impersonate Administrator

Modified critical registry paths in HKLM\SAM

Executed encoded PowerShell to initiate a connection to an external C2 server at 10.10.10.5

Through careful Splunk analysis and forensic event log review, we were able to trace each step of the intrusion.

📁 Skills Demonstrated
Event log correlation in Splunk

Windows security log forensics

Registry analysis

PowerShell attack decoding (Base64, obfuscation)

Threat hunting methodology

