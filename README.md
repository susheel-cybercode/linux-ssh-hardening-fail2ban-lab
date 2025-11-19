# linux-ssh-hardening-fail2ban-lab
SSH Hardening &amp; Brute-Force Attack Detection Lab using Fail2Ban on Lubuntu
🚀 Linux SSH Hardening & Brute-Force Detection Lab

A practical cybersecurity project demonstrating SSH hardening, brute-force attack detection, and automated response using Fail2Ban.

🔥 Overview

This project simulates a real-world brute-force attack using Kali Linux against a Lubuntu server and demonstrates how Fail2Ban detects and blocks malicious SSH login attempts.

The goal is to show practical security hardening, detection engineering, and log analysis skills relevant for SOC, Blue Team, and Threat Intelligence roles.

🖥️ Lab Environment
Component	Details
Target Machine	Lubuntu 20.04 LTS
Attacker Machine	Kali Linux (Host-Only Network)
Tools Used	SSH, UFW, Fail2Ban
Network Type	NAT + Host-Only Adapter
Detection	Fail2Ban SSH jail
Logs	/var/log/auth.log

🛡️ Security Hardening Implemented
🔹 1. Enabled UFW firewall
sudo ufw enable
sudo ufw allow ssh
🔹 2. Installed and configured Fail2Ban
sudo apt install fail2ban -y
🔹 3. Configured SSH jail
/etc/fail2ban/jail.local:
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = /var/log/auth.log
maxretry = 5
findtime = 10m
bantime = 10m
backend = systemd

🚨 Attack Simulation (From Kali)
Repeated SSH login attempts:
ssh wronguser@192.168.56.101
Failing passwords 6–10 times triggered Fail2Ban.

🔍 Detection & Logs
✔ Fail2Ban Status
sudo fail2ban-client status sshd
Shows:
Banned IP list:
192.168.56.102
✔ Auth Log Evidence
Failed password for invalid user test from 192.168.56.102
Ban 192.168.56.102

📊 Network Diagram
[Kali Linux 192.168.56.102]  --->  [Lubuntu Server 192.168.56.101]
            (SSH Bruteforce)        (Fail2Ban Detection & Ban)

📝 Outcome

Brute-force attack successfully detected

Attacker IP automatically banned

SSH service hardened

Logs captured for investigation

This project demonstrates foundational Blue Team and SOC analyst skills including:

✔ Linux hardening
✔ Detection engineering
✔ Log analysis
✔ SSH attack simulation
✔ Real defensive response

📂 Project Files Included

jail.local fail2ban configuration

Log samples (auth.log excerpts)

Attack simulation commands

Screenshots of Fail2Ban status
