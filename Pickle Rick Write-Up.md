#Pickle Rick CTF on TryHackMe

## Introduction
Welcome, fellow hackers! In this write-up, I'll walk you through my approach to solving the **Pickle Rick** CTF challenge on TryHackMe. If you're just getting started with CTFs, this is a great beginner-friendly challenge that covers web exploitation, enumeration, and privilege escalation. 

Let's dive in! 🚀

---

## 🛠️ Step 1: Deploy the Virtual Machine
### **Action:**
- First, I logged into TryHackMe and joined the **Pickle Rick** room.
- I deployed the virtual machine (VM) assigned to the challenge.

### **Tool Used:**
- TryHackMe Platform

---

## 🌐 Step 2: Connecting to the TryHackMe VPN
To interact with the target machine, I needed to establish a VPN connection.

### **Command Used:**
```bash
sudo openvpn [configuration_file].ovpn
```

### **Tool Used:**
- OpenVPN

Once connected, I confirmed my connection by pinging the target IP.

```bash
ping [TARGET_IP]
```

If the ping was successful, I was ready to start scanning. ✅

---

## 🔎 Step 3: Initial Scanning with Nmap
First things first—let’s enumerate the target’s open ports and services using **Nmap**.

### **Command Used:**
```bash
nmap -sC -sV -oN initial_scan.txt [TARGET_IP]
```

### **Findings:**
- Open **port 80 (HTTP)** → Website running.
- Open **port 22 (SSH)** → Possible access point.

### **Tool Used:**
- Nmap

At this point, I knew a website was running, so it was time to explore the web application.

---

## 🌍 Step 4: Exploring the Website
I opened the **Pickle Rick** website in my browser and did the following:
- Checked the **HTML source code**.
- Found a comment with **a username hint** → `Rick rules`.

### **Tools Used:**
- Web browser
- Browser Developer Tools (Inspect Element)

---

## 🔍 Step 5: Directory Enumeration with Gobuster
To uncover hidden directories, I ran **Gobuster**.

### **Command Used:**
```bash
gobuster dir -u http://[TARGET_IP] -w /usr/share/wordlists/dirb/common.txt -x php,html,js
```

### **Findings:**
- Found `/login.php`
- Found `/robots.txt` with a weird phrase: `Wubba lubba dub dub`

### **Tool Used:**
- Gobuster

---

## 🔑 Step 6: Logging into the Web App
I attempted logging in using:
- **Username:** `Rick rules`
- **Password:** `Wubba lubba dub dub`

🎉 **Success!** I was redirected to a **command panel**.

---

## 🛠️ Step 7: Command Execution and File Reading
I tried running commands in the command panel, but some were restricted. However, I used workarounds to read files:

### **Command Used:**
```bash
while read line; do echo $line; done < clue.txt
```
Or:
```bash
grep . clue.txt
```

### **Findings:**
- Discovered **Ingredient 1** inside `clue.txt`.

---

## 🏴 Step 8: Getting a Reverse Shell
Since I had command execution access, I attempted a **reverse shell**.

### **Steps:**
1. **Get a reverse shell script** from **PentestMonkey Reverse Shell Cheat Sheet**.
2. Modify the script with my IP and port.
3. Set up a **Netcat listener**:
    ```bash
    nc -lvnp <PORT>
    ```
4. Upload and execute the script.

🎉 **Shell Access Obtained!**

---

## ⚡ Step 9: Privilege Escalation
Once inside, I scoured directories for further clues.

### **Privilege Escalation Trick:**
Tried running:
```bash
sudo -l
```
Found that I could run `bash` as root! 😲

### **Privilege Escalation Command:**
```bash
sudo bash
```

Now, I was **root**. 🔥

---

## 🎯 Step 10: Capturing the Final Flags
By navigating through the system, I found **Ingredient 2** and **Ingredient 3** in hidden directories.

---

## 📌 Tools Used Summary
| Tool | Purpose |
|------|---------|
| **TryHackMe** | Hosted the CTF challenge |
| **OpenVPN** | Secure connection to TryHackMe |
| **Nmap** | Network scanning and enumeration |
| **Gobuster** | Directory brute-forcing |
| **Nikto** | Web vulnerability scanner |
| **Web Browser & Dev Tools** | Manual website analysis |
| **Netcat (nc)** | Reverse shell connection |
| **Linux Shell Commands** | File manipulation, privilege escalation |

---

## 🔥 Real-Life Lessons from This CTF
- **Enumeration is key.** If one approach fails, try another.
- **Think like an attacker.** Always check `/robots.txt` and source code.
- **Use creative file reading tricks** if `cat` is blocked.
- **Privilege escalation is all about checking what commands you can run.**

---

## 🏁 Conclusion
This was a fun and beginner-friendly CTF that helped reinforce **web exploitation**, **reverse shells**, and **privilege escalation**. If you're learning penetration testing, this is a great exercise to build your hacking skills. 💀💻

---

Hope this write-up helps! Happy hacking! 🚀
