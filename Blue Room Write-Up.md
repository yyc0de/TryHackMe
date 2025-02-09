# Blue Room Write-Up

## Overview

Hey there! In this write-up, I'm going to walk you through the process I followed while working on the **Blue VM** exercise. The goal here is to learn about vulnerability scanning and exploitation techniques in a controlled environment. I’ll be covering the steps I took to identify vulnerabilities, exploit them, escalate my privileges, and eventually capture some flags. So, grab a coffee, and let’s dive in!

---

## Scanning and Identifying Vulnerabilities

First things first, I had to scan the **Blue VM**, which is a Windows-based machine designed for learning purposes. Right off the bat, I noticed the machine doesn’t respond to ping (ICMP), and it might take a bit longer to boot up. 

My task was to identify open ports (less than 1000) and look for known vulnerabilities. One key vulnerability I targeted was **MS17-010**, better known as **EternalBlue**. This vulnerability is widely known for being exploitable via the **Metasploit framework**, which makes identifying and exploiting the vulnerability much easier. So, let me break down the key steps here.

---

## Exploiting a Vulnerability Using Metasploit

Now that I’ve identified a potential vulnerability, it was time to exploit it. The **MS17-010 EternalBlue** vulnerability was a perfect target. Here's how I did it:

1. **Set RHOSTS**: This is where I set the target IP address (RHOSTS) of the **Blue machine**. If you’ve worked with Metasploit before, you know that setting the target IP is crucial.
   
2. **Set Payload**: I needed to choose the right payload. I went with `windows/x64/shell/reverse_tcp`. This payload opens a reverse TCP shell, which is useful for maintaining a connection with the target system.
   
3. **Run Exploit**: After configuring the settings, I ran the exploit. If successful, it gives me access to the target system via a DOS shell. It worked like a charm!

4. **Troubleshooting**: Sometimes, the exploit didn’t work on the first try. If that happens, I simply rebooted the target machine and tried running the exploit again. It’s always a good practice to retry if the initial attempt fails.

---

## Escalating Privileges and Upgrading Shell

Now that I had access to the machine, the next challenge was to **escalate my privileges** and upgrade my shell. Here’s what I did:

1. **Background the Shell**: After gaining access, I pressed `CTRL + Z` to background my current shell session. This frees me up to do other tasks without losing the connection.
   
2. **Convert to Meterpreter**: This was a crucial step. I used the Metasploit post-exploitation module `post/multi/manage/shell_to_meterpreter` to convert my shell into a **Meterpreter** session. Why Meterpreter? It’s a more powerful shell that offers advanced features for further exploitation.
   
3. **Verify Privileges**: To confirm that I successfully escalated my privileges, I ran the `getsystem` command. This command verifies that I’m running with **NT AUTHORITY\SYSTEM** privileges. I also ran the `whoami` command just to double-check that I was indeed elevated.

4. **Migrate to SYSTEM Process**: With **SYSTEM** privileges, I wanted to make my session more stable. To do that, I migrated to a process running under **NT AUTHORITY\SYSTEM**. I identified a running process and used its process ID (PID) to migrate. If migration fails, try another process or repeat the steps.

---

## Dumping and Cracking Passwords

By now, I had elevated privileges, and it was time to move on to **dumping passwords** from the machine. Here’s what I did:

1. **Dump Passwords**: Using the elevated **Meterpreter** session, I ran the `hashdump` command. This command dumps all the system's password hashes. It’s a great way to collect sensitive data for further exploitation.
   
2. **Identify Non-Default User**: After dumping the hashes, I analyzed them and identified a non-default user: **Jon**. Identifying users who are not part of the default setup is an important part of the process. It can indicate that these users may have special privileges or other useful information.
   
3. **Crack Password**: I took the password hash for **Jon**, saved it to a file, and then cracked it using hash-cracking tools. After some time, I cracked the password: `alqfna22`.

---

## Finding Flags on the Windows Machine

At this point, the fun part of the exercise came: **finding the flags**. Here’s where the system holds some interesting secrets:

- **Flag 1**: Found in the system root directory.  
  `flag{access_the_machine}` – This was the first flag, and it’s basically an indication that you’ve gained access.

- **Flag 2**: Located in the **SAM** database (where Windows stores password hashes). This flag is a bit tricky because Windows deletes it upon system restart. So, I had to reboot the machine to recover it.  
  `flag{sam_database_elevated_access}` – This was a cool find since it demonstrated that I had elevated access to the SAM database.

- **Flag 3**: This one was in the **administrator’s directory**, where important documents are stored.  
  `flag{admin_documents_can_be_valuable}` – A good reminder that admin folders can hold some interesting (and useful) data!

---

## Conclusion

So, that’s how I tackled the **Blue VM** exercise! This was a great hands-on approach to learning about vulnerability scanning, exploitation, privilege escalation, and cracking passwords on a Windows machine. The **Blue VM** is perfect for beginners, and it really helped me understand common attack vectors like **EternalBlue**, **privilege escalation**, and **password cracking**.

By now, you should have a good understanding of the techniques involved in **penetration testing**. As you move on to more complex environments, these foundational skills will come in handy. If you’re just starting with penetration testing, this is an excellent first step!

Happy hacking, and I hope this guide helps you in your learning journey!

---

## Resources

- **TryHackMe**: [Blue VM Room](https://www.tryhackme.com)
- **Metasploit Framework**: [Metasploit Official Website](https://www.metasploit.com)
- **Nmap**: [Nmap Official Website](https://nmap.org)

---

*Good luck, and keep learning!*
