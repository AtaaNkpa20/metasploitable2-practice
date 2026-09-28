# Metasploitable 2 — Penetration Testing Practice Lab

A personal lab documenting my hands-on penetration testing practice using Kali Linux and Metasploitable 2. This repo covers network scanning, service exploitation, manual exploitation, and web application attacks.

> **Disclaimer:** All activity was performed in an isolated, controlled virtual lab environment. Never attempt these techniques on systems you do not own or have explicit permission to test.

---

## Lab Environment

| Component | Details |
|-----------|---------|
| Attacker | Kali Linux (VirtualBox VM) |
| Target | Metasploitable 2 (VirtualBox VM) |
| Network | Host-only Adapter (isolated) |
| Attacker IP | 192.168.56.105 |
| Target IP | 192.168.56.106 |

---

## Setup

- Both VMs run in **VirtualBox** on a **Host-only network** (`vboxnet0`)
- This isolates all traffic to the host machine — nothing touches the real network
- Metasploitable 2 login: `msfadmin / msfadmin`

---

## Index

| # | Topic | File |
|---|-------|------|
| 1 | Network Scanning with Nmap | [recon/nmap-scan.md](recon/nmap-scan.md) |
| 2 | vsftpd 2.3.4 Backdoor (Metasploit) | [exploitation/vsftpd-metasploit.md](exploitation/vsftpd-metasploit.md) |
| 3 | vsftpd 2.3.4 Backdoor (Manual) | [exploitation/vsftpd-manual.md](exploitation/vsftpd-manual.md) |
| 4 | Samba usermap_script RCE | [exploitation/samba-usermap.md](exploitation/samba-usermap.md) |
| 5 | SQL Injection — DVWA | [web-attacks/sql-injection.md](web-attacks/sql-injection.md) |
| 6 | Password Hash Cracking | [web-attacks/password-cracking.md](web-attacks/password-cracking.md) |

---

## Skills Covered

- Network reconnaissance with Nmap
- Service version detection
- Exploit frameworks (Metasploit)
- Manual exploitation with Netcat
- SQL injection (UNION-based)
- MD5 hash cracking with John the Ripper
