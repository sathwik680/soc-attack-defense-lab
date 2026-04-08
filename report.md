# 📊 SOC Attack & Defense Detailed Report

## 📌 Introduction

This report documents a hands-on cybersecurity lab where an SMB-based attack was simulated and analyzed from both attacker and defender perspectives.

---

## 🧪 Lab Environment

* Attacker Machine: Kali Linux
* Target Machine: Windows 10
* Network: Host-only (192.168.56.x)

---

## 🔴 Attack Execution

### 1. Reconnaissance

Performed service detection using Nmap:

```bash
nmap -sV 192.168.56.103
```

#### Findings:

* Port 135 (RPC)
* Port 139 (NetBIOS)
* Port 445 (SMB)

👉 SMB identified as primary attack vector.

---

### 2. SMB Enumeration

```bash
nmap --script smb-enum-shares -p445 192.168.56.103
```

#### Observation:

* No anonymous shares available
* Access restricted

---

### 3. Vulnerability Assessment

```bash
nmap --script smb-vuln* -p445 192.168.56.103
```

#### Observation:

* No critical vulnerabilities found
* System hardened against known exploits

---

### 4. Authentication Attack Simulation

```bash
smbclient -L //192.168.56.103 -U admin
```

#### Actions:

* Multiple incorrect password attempts
* One successful authentication

#### Result:

* Generated authentication logs on target system

---

## 🔵 Detection & Analysis

### Tool Used:

* Windows Event Viewer

---

### Log Path:

```
Windows Logs → Security
```

---

### Key Events Identified:

#### 🔹 Event ID 4625 – Failed Login

* Indicates incorrect login attempts
* Suggests brute-force behavior

---

#### 🔹 Event ID 4624 – Successful Login

* Confirms valid authentication
* Marks successful access

---

#### 🔹 Event ID 4672 – Privileged Login

* Indicates administrative privileges assigned
* High-risk event in SOC monitoring

---

### 🧠 Analysis

* Multiple failed login attempts indicate suspicious activity
* Successful login followed by privilege assignment increases risk level
* Source IP (Kali Linux) confirms attack origin

---

## 🚨 Security Implications

* SMB services are common attack targets
* Weak or exposed credentials can lead to unauthorized access
* Monitoring authentication logs is critical for early detection

---

## 🧠 Key Learnings

* Practical understanding of SMB protocol behavior
* Hands-on experience with enumeration and authentication attacks
* Ability to detect attacks using Windows logs
* Understanding of SOC workflows (Detection → Analysis → Response)

---

## 🚀 Conclusion

This lab successfully demonstrates how attackers exploit authentication mechanisms and how defenders can detect such activity using system logs. It highlights the importance of monitoring and securing SMB services in real-world environments.
