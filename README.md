# 🛡️ Linux SSH Hardening & Fail2Ban Brute-Force Detection Lab

## 🔹 TL;DR
Simulated SSH brute-force attacks from Kali → Lubuntu.  
Fail2Ban detected repeated authentication failures and automatically **banned the attacker IP**.  
This repo includes configs, logs, screenshots, and full reproduction steps.  
A strong Blue-Team / SOC / Cloud Security portfolio project.

---

# ⚙️ Project Overview
This lab demonstrates practical **Linux security hardening**, **Blue Team detection**, and **automated response** using Fail2Ban.

Workflow:
1. Kali performs SSH brute-force attempts  
2. Lubuntu monitors `/var/log/auth.log`  
3. Fail2Ban detects the pattern  
4. Offending IP is **automatically banned**  
5. Logs and evidence are collected for analysis

---

# 🧪 Lab Architecture
```
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
```

---

# 🔧 Tools & Technologies Used
| Tool | Purpose |
|------|---------|
| **Fail2Ban** | Detect & block brute-force attacks |
| **OpenSSH Server** | Attack surface |
| **UFW Firewall** | Basic Linux firewall |
| **Nmap** | Recon & port scanning |
| **Lubuntu** | Target server |
| **Kali Linux** | Attacker |

---

# 🗂️ Repository Contents
| File / Folder | Description |
|---------------|-------------|
| `jail.local` | Fail2Ban SSH jail configuration |
| `auth-log-snippet.txt` | Evidence of failed logins, bans, unbans |
| `commands-used.txt` | Commands used in simulation & configuration |
| `/screenshots` | Visual evidence |
| `README.md` | Full documentation |

---

# 🔒 Fail2Ban Configuration
Location: `/etc/fail2ban/jail.local`  
Included in this repo.

```
maxretry = 5
findtime = 10m
bantime = 10m
logpath = /var/log/auth.log
backend = systemd
```

---

# 🚀 Attack Simulation
### SSH brute-force from Kali
```
ssh wronguser@192.168.56.101
# Repeat wrong password
```
### Recon with Nmap
```
nmap -sS 192.168.56.101
```
Full command list in `commands-used.txt`.

---

# 📄 Log Evidence
Real log lines stored in `auth-log-snippet.txt`.

Examples:
```
Failed password for invalid user wronguser from 192.168.56.101
Ban 192.168.56.101
Unban 192.168.56.101
```

---

# 🖼️ Screenshots / Evidence
Refer to the `/screenshots` folder.

```
fail2ban-status.png
kali-ssh-attacks.png
ufw-status.png
auth-log.png
```

---

# 🧠 MITRE ATT&CK Mapping
| Behavior | Technique |
|----------|-----------|
| SSH brute-force | T1110 – Brute Force |
| Repeated authentication failures | T1078 – Valid Accounts |
| Nmap SYN scan | T1046 – Network Service Scanning |
| Log-based detection | T1057 – Process Monitoring |

---

# 🔁 How to Reproduce This Lab
## 1️⃣ VirtualBox Networking
Enable:
- **NAT** (internet)
- **Host-Only Adapter (vboxnet0)**

IP examples:
- Lubuntu: `192.168.56.101`
- Kali: `192.168.56.102`

---

## 2️⃣ Prepare Lubuntu (Target)
```
sudo apt update
sudo apt install -y ufw fail2ban openssh-server
sudo ufw enable
sudo ufw allow ssh
```
Copy jail config:
```
sudo cp jail.local /etc/fail2ban/jail.local
sudo systemctl restart fail2ban
```

---

## 3️⃣ Attack from Kali
```
ssh wronguser@192.168.56.101
nmap -sS 192.168.56.101
```

---

## 4️⃣ Verify Detection
```
sudo fail2ban-client status sshd
sudo tail -n 20 /var/log/auth.log
sudo ufw status numbered
```

---

# 📊 Skills Demonstrated
✔ Linux Hardening  
✔ SSH Security  
✔ Log Analysis  
✔ Detection Engineering  
✔ Brute-Force Detection  
✔ Evidence Gathering  
✔ Blue Team Workflow  
✔ SOC Investigation

---

# 📎 License
MIT License

---

# 👤 Author
**Susheel M S**  
Cybersecurity Analyst (SOC / Blue Team / Threat Intelligence)
GitHub: https://github.com/susheel-cybercode
