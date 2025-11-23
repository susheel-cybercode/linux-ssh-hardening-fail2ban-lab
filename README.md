# 🛡️ Linux SSH Hardening & Fail2Ban Brute-Force Detection Lab

## TL;DR
Simulated SSH brute-force attacks from Kali → Lubuntu.  
Fail2Ban detected repeated authentication failures and automatically **banned the attacker IP**.  
Includes configs, logs, screenshots, and reproducible lab steps.

---

# 📌 Project Overview

This project demonstrates practical **Linux security hardening**, **attack simulation**, and **automated detection & response** using Fail2Ban.  
The lab replicates a real-world SOC workflow:

- Attacker performs SSH brute-force  
- Defender (Fail2Ban) monitors `/var/log/auth.log`  
- Fail2Ban detects abnormal patterns  
- Attacker IP is automatically **banned**  
- Evidence is collected and analyzed  

This is valuable for **SOC Analyst**, **Blue Team**, **Threat Intelligence**, and **Security Engineer** roles.

---

# 🧪 Lab Architecture

        +--------------------------+
        |      Kali Linux          |
        |   Attacker: 192.168.56.x |
        +------------+-------------+
                     |
             Host-Only (vboxnet0)
                     |
        +------------+-------------+
        |     Lubuntu Server       |
        |  Target: 192.168.56.x    |
        |  Fail2Ban + SSH Hardening|
        +---------------------------+

**Lubuntu (Target):**
- UFW Firewall  
- SSH Server  
- Fail2Ban (sshd jail)  

**Kali (Attacker):**
- SSH brute-force attempts  
- Recon using Nmap  

---

# ⚙️ Tools & Technologies

| Component | Purpose |
|----------|---------|
| **Fail2Ban** | Brute-force detection & automatic banning |
| **UFW** | Basic firewall rules |
| **SSH** | Attack surface |
| **Nmap** | Recon & scanning |
| **Lubuntu** | Linux server target |
| **Kali Linux** | Attacker machine |

---

# 🔧 Fail2Ban Configuration (`jail.local`)

Stored in this repo:  
`jail.local`

Key values:

- `maxretry = 5`  
- `findtime = 10m`  
- `bantime = 10m`  
- `backend = systemd`  
- Log monitored: `/var/log/auth.log`

---

# 🚀 Attack Simulation Steps

Full commands included in:  
`commands-used.txt`

### **1️⃣ Kali attacks Lubuntu through SSH**

```bash
ssh wronguser@192.168.56.101
# Repeat wrong password attempts 6–10 times

2️⃣ Fail2Ban detects repeated failures

Monitors:
/var/log/auth.log

3️⃣ IP is banned automatically

Check ban status:
sudo fail2ban-client status sshd

📄 Log Evidence

All log samples stored in:
auth-log-snippet.txt

Includes:

Failed SSH login attempts

Multiple retry patterns

Fail2Ban warnings

Ban & unban events

Example log:
Failed password for invalid user wronguser from 192.168.56.101
Ban 192.168.56.101
Unban 192.168.56.101

🖼️ Screenshots / Evidence
🔹 Fail2Ban banning attacker IP

🔹 Kali brute-force SSH attempts

🔹 UFW firewall configuration

🔹 Authentication log evidence

📚 MITRE ATT&CK Mapping
Behavior	Technique
SSH brute-force attempts	T1110 – Brute Force
Repeated failed authentication	T1078 – Valid Accounts (Misuse)
Nmap reconnaissance	T1046 – Network Service Scanning
Automated blocking	Detection Engineering (Custom)

🔁 How to Reproduce This Lab
1. Setup VirtualBox Network

Enable Host-Only Adapter (vboxnet0)

Assign:

VM	IP
Lubuntu	192.168.56.101
Kali	192.168.56.102

sudo apt update
sudo apt install -y ufw fail2ban openssh-server
sudo ufw enable
sudo ufw allow ssh
sudo systemctl restart fail2ban
Copy the repo's jail.local → /etc/fail2ban/jail.local

3. Attack from Kali
ssh wronguser@192.168.56.101
nmap -sS 192.168.56.101

4. Verify ban on Lubuntu
sudo fail2ban-client status sshd
sudo tail -n 20 /var/log/auth.log

🧠 What This Project Demonstrates

✔ Linux hardening
✔ SSH security
✔ Brute-force detection
✔ Log analysis
✔ Fail2Ban configuration
✔ Defensive automation
✔ Blue-team thinking
✔ Evidence collection

This is a job-ready project for SOC / Blue Team portfolios.

📝 Files in This Repository
File	Description
jail.local	Fail2Ban configuration
auth-log-snippet.txt	Attack & detection logs
commands-used.txt	All commands used in lab
screenshots/	Visual proof of attack & detection
README.md	Full project documentation

📎 License

MIT License
