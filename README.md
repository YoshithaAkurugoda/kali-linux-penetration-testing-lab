# Kali Linux Penetration Testing Lab

This project demonstrates a structured penetration testing exercise performed in a controlled virtual lab environment using Kali Linux.

The objective was to simulate a real-world attack chain, starting from reconnaissance and progressing through multiple exploitation stages until full root access was obtained.

The lab environment consisted of:

- Kali Linux attacker machine
- Vulnerable Linux target machine
- Internal subnet environment

---

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

---

## Attack Phases

### Phase 1 – Reconnaissance

Tools used:

- Nmap

Commands used:
ip a
nmap 192.168.0.0/24 -sP
nmap -sV 192.168.0.100
