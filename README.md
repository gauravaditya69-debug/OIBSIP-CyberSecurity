# 🔍 Task 1: Basic Network Scanning with Nmap

> Oasis Infobyte – Cybersecurity Internship (1 Month)  
> Intern: Aditya Gaurav  
> Date: July 12, 2026

---

## 👋 Introduction & Objective

Hi! Welcome to my repository for Task 1 of the Oasis Infobyte Cybersecurity Internship. 

For this task, my goal was to get hands-on experience with Nmap (Network Mapper). I wanted to perform a basic network scan to discover open ports, identify the services running on them, and figure out exactly what versions of those services were deployed on a target machine. This kind of reconnaissance is step one in any vulnerability assessment or penetration test.

---

## 🛠️ What I Used

Here is the setup I used for this task:

| Tool/Environment | Details |
|------|---------|
| Nmap | Version 7.99 (The star of the show!) |
| Npcap | Version 1.87 (Required for packet capture on Windows) |
| Terminal | Windows PowerShell |

---

## 🎯 The Target

For safety and legality, I didn't want to scan random servers on the internet. Instead, I used the official test server provided by the creators of Nmap:

- Hostname: `scanme.nmap.org`
- IP Address: `45.33.32.156`

*Disclaimer: `scanme.nmap.org` is a server explicitly set up to allow security enthusiasts to practice their scanning skills legally.*

---

## 🚀 How I Ran the Scans

I split the scanning process into two distinct steps to see how the target would respond.

### 1. The "Are you there?" Ping Scan

First, I just wanted to see if the server was alive without actually probing any of its ports. 

```bash
nmap -sn scanme.nmap.org
```

What happened: The server responded almost immediately! The ping scan confirmed the host was UP with a very quick latency of 0.26 seconds. It took Nmap just 1.13 seconds to figure this out.

### 2. The Deep Dive (Service & Version Detection)

Next, I wanted the details. I asked Nmap to not only find open ports but also tell me exactly what services were running on them.

```bash
nmap -sV -sC -T4 scanme.nmap.org
```

Why these flags?
- `-sV` tells Nmap to figure out the service versions.
- `-sC` runs default Nmap scripts to dig a little deeper.
- `-T4` speeds up the scan so I don't have to wait all day.

---

## 📊 What I Found (The Results)

Out of the 1,000 common ports that Nmap checks by default, 995 were firmly closed. That left 5 ports that were doing something interesting:

| Port | State | Service | Version Discovered |
|------|-------|---------|--------------------|
| 22 | 🟢 Open | SSH | OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 |
| 25 | 🟡 Filtered | SMTP | *Blocked by a firewall* |
| 80 | 🟢 Open | HTTP | Apache httpd 2.4.7 (Ubuntu) |
| 9929 | 🟢 Open | Nping Echo | Nping echo service |
| 31337 | 🟢 Open | TCPWrapped | *Connection accepted but then dropped* |

---

## 🔎 My Analysis & Security Thoughts

Here is my breakdown of what these open ports actually mean from a security perspective:

### 1. Port 22 (SSH)
Finding SSH open isn't unusual, as administrators need a way to manage the server remotely. However, the version running here (OpenSSH 6.6.1p1) is quite old! In a real-world scenario, I'd highly recommend patching this to the latest version to avoid known vulnerabilities. I also noticed it's using a 1024-bit DSA key, which is considered weak by modern standards.

### 2. Port 25 (SMTP)
This port handles email routing. Interestingly, Nmap reported it as Filtered. This means there is a firewall sitting in front of it blocking my probes. This is actually a great security practice—servers shouldn't leave SMTP wide open to the public unless they are actively functioning as public mail servers.

### 3. Port 80 (HTTP)
The web server is running on port 80 using Apache 2.4.7. Just like the SSH service, this version is pretty outdated. Another thing to note is that port 80 is unencrypted HTTP. There was no port 443 (HTTPS) open, meaning any traffic sent to this web page is sent in plain text.

### 4. Port 9929 (Nping Echo)
This one is a bit unique. It's a testing service set up specifically by the Nmap team so people can practice packet generation and response analysis. In a normal company environment, you'd definitely want to turn this off.

### 5. Port 31337 (TCPWrapped)
This port stood out to me. The number 31337 is infamous in cybersecurity history as it was commonly used by old backdoor trojans (like Back Orifice). Nmap marked it as `tcpwrapped`, which basically means the server completes the initial connection handshake but then immediately closes it without sending any real data. If I saw this on a real client's network, it would definitely warrant a serious investigation.

---

## 📁 Files in this Repository

- `nmap_scan_results.txt`: The raw text output of the scans I ran.
- `README.md`: The document you are reading right now!

---

Thanks for checking out my work for Task 1!

*— Aditya Gaurav*
