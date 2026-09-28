# Network Scanning with Nmap

## Objective
Identify open ports, running services, and software versions on the target machine before attempting any exploitation.

---

## Commands Used

### Basic scan
```bash
nmap 192.168.56.106
```
Confirms the host is up and lists open TCP ports.

### Version detection scan
```bash
nmap -sV 192.168.56.106
```
The `-sV` flag probes each open port to identify the service name and version number.

---

## Results

**Host:** 192.168.56.106  
**Status:** Up (0.00036s latency)  
**Closed ports:** 977

| Port | State | Service | Version |
|------|-------|---------|---------|
| 21/tcp | open | ftp | vsftpd 2.3.4 |
| 22/tcp | open | ssh | OpenSSH 4.7p1 Debian 8ubuntu1 |
| 23/tcp | open | telnet | Linux telnetd |
| 25/tcp | open | smtp | Postfix smtpd |
| 53/tcp | open | domain | ISC BIND 9.4.2 |
| 80/tcp | open | http | Apache httpd 2.2.8 (Ubuntu) DAV/2 |
| 111/tcp | open | rpcbind | 2 (RPC #100000) |
| 139/tcp | open | netbios-ssn | Samba smbd 3.X-4.X |
| 445/tcp | open | netbios-ssn | Samba smbd 3.X-4.X |
| 512/tcp | open | exec | netkit-rsh rexecd |
| 513/tcp | open | login | OpenBSD or Solaris rlogind |
| 514/tcp | open | shell | Netkit rshd |
| 1099/tcp | open | java-rmi | GNU Classpath grmiregistry |
| 1524/tcp | open | bindshell | Metasploitable root shell |
| 2049/tcp | open | nfs | 2-4 (RPC #100003) |
| 2121/tcp | open | ftp | ProFTPD 1.3.1 |
| 3306/tcp | open | mysql | MySQL 5.0.51a-3ubuntu5 |
| 5432/tcp | open | postgresql | PostgreSQL DB 8.3.0-8.3.7 |
| 5900/tcp | open | vnc | VNC protocol 3.3 |
| 6000/tcp | open | X11 | access denied |
| 6667/tcp | open | irc | UnrealIRCd |
| 8009/tcp | open | ajp13 | — |
| 8180/tcp | open | http | Apache Tomcat/Coyote JSP engine 1.1 |

---

## Key Observations

- **vsftpd 2.3.4** on port 21 — this exact version has a known backdoor
- **Samba** on ports 139/445 — vulnerable to usermap_script RCE
- **UnrealIRCd** on port 6667 — also contains a known backdoor
- **MySQL/PostgreSQL** — databases exposed with weak/default credentials
- **Port 1524** — Metasploitable intentionally leaves a bindshell open here
- The MAC address (`08:00:27:9D:C6:D7`) confirms this is a VirtualBox VM on the local network segment

---

## What I Learned

- Nmap `-sV` reveals software versions, which are critical for finding matching CVEs
- A machine with this many open ports and outdated services would be an immediate red flag in a real assessment
- The order of attack starts here: scan first, then research what each version is vulnerable to before touching anything
