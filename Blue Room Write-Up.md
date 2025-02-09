# Blue Room Write-Up

## Introduction

Hey there! 👋 In this write-up, I’ll take you through the process I followed to scan, exploit, and escalate privileges on the **Blue** virtual machine on **TryHackMe**. This exercise is perfect for beginners looking to learn how to find and exploit vulnerabilities in a Windows-based machine.

### What We Will Cover:
- Scanning for open ports and vulnerabilities
- Exploiting the **EternalBlue** vulnerability
- Escalating privileges and upgrading the shell
- Dumping and cracking passwords
- Finding flags on the Windows machine

Let’s dive in! 🏃‍♂️

---

## Scanning and Identifying Vulnerabilities

When I first started, I noticed that the **Blue** machine didn’t respond to **ping (ICMP)**, and it took a bit to boot up. I focused on scanning for open ports under **1000**, as specified in the task.

During my scan, I paid special attention to **MS17-010** (**EternalBlue**), a well-known vulnerability that affects SMBv1. This gave me clues about potential exploits.

🔍 **Scanning for open ports** and **identifying vulnerabilities** is crucial in penetration testing, as it helps you map out the attack surface and understand what you’re dealing with.

---

## Exploiting the Vulnerability Using Metasploit

Next, I moved on to exploiting the system using **Metasploit**. Here's the step-by-step process:

### 1. Set RHOSTS
Set the **RHOSTS** value to the target machine’s IP address. This tells Metasploit where to direct the exploit.

```bash
set RHOSTS <target_ip>
