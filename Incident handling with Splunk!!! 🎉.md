Conclusion:

In this fun exercise, as a SOC Analyst, we have investigated a cyber-attack where the attacker had defaced a website 'imreallynotbatman.com' of the Wayne Enterprise. We mapped the attacker's activities into the 7 phases of the Cyber Kill Chain. Let us recap everything we have found so far:

Reconnaissance Phase:

We first looked at any reconnaissance activity from the attacker to identify the IP address and other details about the adversary.

Findings:

IP Address 40.80.148.42 was found to be scanning our webserver.
The attacker was using Acunetix as a web scanner.
Exploitation Phase:

We then looked into the traces of exploitation attempts and found brute-force attacks against our server, which were successful.

Findings:

Brute force attack originated from IP 23.22.63.114.
The IP address used to gain access: 40.80.148.42
142 unique brute force attempts were made against the server, out of which one attempt was successful
Installation Phase:

Next, we looked at the installation phase to see any executable from the attacker's IP Address uploaded to our server.

Findings:

A malicious executable file 3791.exe was observed to be uploaded by the attacker.
We looked at the sysmon logs and found the MD5 hash of the file.
Action on Objective:

After compromising the web server, the attacker defaced the website.

Findings:

We examined the logs and found the file name used to deface the webserver.
Weaponization Phase:

We used various threat Intel platforms to find the attacker's infrastructure based on the following information we saw in the above activities.

Information we had:

Domain: prankglassinebracket.jumpingcrab.com

IP Address: 23.22.63.114
Findings:

Multiple masquerading domains were found associated with the attacker's IPs.
An email of the user Lillian.rose@po1s0n1vy.com was also found associated with the attacker's IP address.
Deliver Phase:

In this phase, we again leveraged online Threat Intel sites to find malware associated with the adversary's IP address, which appeared to be a secondary attack vector if the initial compromise failed.

Findings:

A malware name MirandaTateScreensaver.scr.exe was found associated with the adversary.
MD5 of the malware was c99131e0169171935c5ac32615ed6261
