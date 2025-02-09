Blue Room Write-Up
Introduction
Hey there! In this write-up, I’ll take you through the process I followed to scan, exploit, and escalate privileges on the Blue virtual machine on TryHackMe. The purpose of this exercise is to learn how to find and exploit vulnerabilities in a Windows-based machine. If you're new to ethical hacking or penetration testing, this is a great place to start. I’ll guide you through the steps, so you can follow along and gain hands-on experience.

Scanning and Identifying Vulnerabilities
When I started, the Blue machine wasn’t responding to pings (ICMP), and it took a little while to boot up. I initially focused on scanning for open ports. Since the task specified scanning for ports under 1000, I ran a basic port scan to see which ones were open. During this process, I also kept an eye on the known vulnerabilities, particularly MS17-010, which is associated with EternalBlue.

This is a great example of how vulnerability scanning works in real-world penetration tests. It's not just about finding a machine but understanding its weaknesses based on things like open ports and known security flaws.

Exploiting the Vulnerability Using Metasploit
Next, I moved on to exploiting the system using Metasploit and the MS17-010 (EternalBlue) exploit. Here’s what I did:

Set RHOSTS: First, I set the RHOSTS variable to the target machine’s IP. This tells Metasploit where to direct the exploit.

Set Payload: I used a reverse TCP shell as the payload (windows/x64/shell/reverse_tcp), which allows me to get a shell back on my local machine when the exploit succeeds.

Run the Exploit: After setting everything up, I ran the exploit. Once it was successful, I gained access to the target system. You’ll notice that if the exploit runs successfully, it will drop you into a DOS shell, giving you access to the system.

Troubleshooting: If the exploit fails (as it did for me once), I simply rebooted the target machine and ran the exploit again. It worked like a charm the second time!

By using Metasploit, I was able to take advantage of an unpatched vulnerability and access the target machine with ease. This is one of the core techniques in ethical hacking, as vulnerabilities like EternalBlue are widely exploited in the wild.

Escalating Privileges and Upgrading Shell
After gaining initial access, the next step was privilege escalation. Here's how I did it:

Background the Shell: I hit CTRL + Z to background the shell and move on to upgrading it.

Convert to Meterpreter: I used the post/multi/manage/shell_to_meterpreter module in Metasploit to convert my shell to a Meterpreter session. Meterpreter is a powerful tool because it gives you more control over the machine compared to a regular shell.

Verify Privileges: I used the getsystem command to escalate privileges to NT AUTHORITY\SYSTEM, which is the highest user privilege level on Windows. I also ran whoami to verify that I had the necessary privileges.

Migrate to SYSTEM Process: After obtaining elevated privileges, I used the ps command to list running processes. I found one running under NT AUTHORITY\SYSTEM and migrated my session to that process. If migration fails, don’t worry! Just retry it, or use a different process.

This step was key because once I had SYSTEM-level privileges, I could do almost anything on the target machine. Privilege escalation is often where the real fun begins in penetration testing.

Dumping and Cracking Passwords
At this stage, I had full access, so it was time to dump the passwords from the system:

Dump Passwords: I ran the hashdump command within the Meterpreter session. This command dumps all the password hashes on the system, which is useful for cracking passwords.

Identify the Non-Default User: From the dumped hashes, I identified that the non-default user on the system was Jon.

Crack the Password: I copied Jon’s password hash and cracked it using hash-cracking tools. The cracked password turned out to be alqfna22.

Cracking passwords is an essential skill in penetration testing, as it allows you to authenticate as legitimate users on a system. In this case, cracking Jon's password helped me gather valuable information for further privilege escalation.

Finding Flags on the Windows Machine
Now that I had full control of the machine, my final task was to find the flags. Here’s where they were hidden:

Flag 1: This was located at the system root directory. I found it and retrieved it: flag{access_the_machine}.

Flag 2: The second flag was stored in the SAM database, where Windows keeps password hashes. Sometimes, Windows may delete this flag, but after restarting the machine, I was able to retrieve it: flag{sam_database_elevated_access}.

Flag 3: The final flag was in a directory where administrators often store important files. After some searching, I found it: flag{admin_documents_can_be_valuable}.

Flags are great markers in CTF (Capture The Flag) exercises. Each one represents a key accomplishment, and finding them is a solid indicator that you’ve successfully exploited the machine.

Conclusion
That’s a wrap on the Blue room! From scanning and exploiting the machine to escalating privileges and finding flags, this exercise really helped me hone my skills in penetration testing. The process covered many important aspects, like exploiting vulnerabilities (EternalBlue), escalating privileges, cracking passwords, and finding flags.

If you’re just getting started in cybersecurity, I highly recommend you check out the Blue room on TryHackMe. It’s a perfect way to get hands-on experience and understand the basics of ethical hacking. If you’re looking for more advanced challenges, check out the follow-up rooms, Ice and Blaster!

By the way, don’t hesitate to revisit the steps, try different tools, or experiment with various payloads. Penetration testing is all about learning and adapting, so keep practicing! Feel free to reach out if you have any questions or need help with the challenges.

Good luck, and happy hacking! 👨‍💻💻