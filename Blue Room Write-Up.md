# Blue Room Write-Up

## Overview

The Blue virtual machine is designed for beginners to learn about vulnerability scanning and exploitation techniques in a controlled environment. In this exercise, the goal was to identify vulnerabilities in the system, exploit them, and escalate privileges to gain deeper access. This write-up provides a comprehensive overview of the steps taken to accomplish the task using various tools and techniques.

---

## Scanning and Identifying Vulnerabilities

The Blue VM is a Windows-based machine that is designed for educational use. When performing an initial scan, the machine does not respond to ping (ICMP) and may take time to boot. The task involves identifying open ports (less than 1000) and known vulnerabilities, such as **MS17-010** (related to EternalBlue). This vulnerability can be exploited using the **Metasploit framework**, which allows for easy identification and exploitation of the system.

---

## Exploiting a Vulnerability Using Metasploit

To exploit the machine, Metasploit is used with the **MS17-010 EternalBlue** vulnerability. The steps include:

1. **Set RHOSTS**: Set the target IP address (RHOSTS) of the Blue machine.
2. **Set Payload**: Set the payload to `windows/x64/shell/reverse_tcp`.
3. **Run Exploit**: Execute the exploit. Upon success, access is gained to the target system through a DOS shell.
4. **Troubleshooting**: If the exploit fails, reboot the target machine and try running the exploit again.

---

## Escalating Privileges and Upgrading Shell

After successfully exploiting the target, the next step is to escalate privileges and upgrade the shell. The steps taken were:

1. **Background the Shell**: Press `CTRL + Z` to background the current shell session.
2. **Convert to Meterpreter**: Use the Metasploit post-exploitation module `post/multi/manage/shell_to_meterpreter` to convert the shell into a Meterpreter session. The correct session is selected by listing active sessions.
3. **Verify Privileges**: After upgrading, run the `getsystem` command to confirm the escalation to `NT AUTHORITY\SYSTEM`. Use the `whoami` command to verify the user identity.
4. **Migrate to SYSTEM Process**: Find a process running under `NT AUTHORITY\SYSTEM`, note the process ID (PID), and migrate to this process for persistence. If migration fails, try a different process or repeat the upgrade.

---

## Dumping and Cracking Passwords

Once elevated privileges are obtained, the next step is to dump and crack passwords from the target machine:

1. **Dump Passwords**: Using the elevated Meterpreter session, run the `hashdump` command to dump the system’s password hashes.
2. **Identify Non-Default User**: From the dumped passwords, identify the non-default user. In this case, it was **Jon**.
3. **Crack Password**: The password hash is copied to a file and cracked using hash-cracking tools. The cracked password for Jon was `alqfna22`.

---

## Finding Flags on the Windows Machine

The final goal in this exercise was to locate various flags on the machine:

- **Flag 1**: Found in the system root directory.  
  `flag{access_the_machine}`

- **Flag 2**: Located in the SAM database where Windows stores password hashes. This flag is deleted by Windows upon system restart, so the system needs to be rebooted to recover it.  
  `flag{sam_database_elevated_access}`

- **Flag 3**: Found in the administrator’s directory where important documents are stored.  
  `flag{admin_documents_can_be_valuable}`

---

## Conclusion

This exercise provided a hands-on approach to scanning, exploiting vulnerabilities, escalating privileges, and capturing flags on a Windows machine. The **Blue VM** is an excellent resource for beginners to get acquainted with common attack vectors, including **EternalBlue**, privilege escalation, and password cracking. The skills learned here can be applied in more complex environments and form a foundational understanding of penetration testing techniques.

---

## Resources

- **TryHackMe**: [Blue VM Room](https://www.tryhackme.com)
- **Metasploit Framework**: [Metasploit Official Website](https://www.metasploit.com)
- **Nmap**: [Nmap Official Website](https://nmap.org)

---

*Happy Hacking!*

