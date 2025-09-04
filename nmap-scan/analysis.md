# Nmap Scan Analysis – Metasploitable 2

## Objective
The goal of this exercise was to perform a network scan against the Metasploitable 2 virtual machine using Nmap to identify open ports, running services, and potential vulnerabilities.

## Scan Details
- **Tool:** Nmap
- **Command used:** `nmap -A <Metasploitable_IP> -oN scan-results.txt`
- **Date:** 9/2/2025
- **Target:** Metasploitable 2 VM (intentionally vulnerable system)

## Key Findings
**Open Ports Detected:**

- **21/tcp – FTP (vsftpd 2.3.4)**  
  - Vulnerable to a known backdoor exploit ([CVE-2011-2523](https://nvd.nist.gov/vuln/detail/CVE-2011-2523))  
  - Anonymous FTP login allowed, permitting unauthorized access  
  - All FTP commands and credentials transmitted in plaintext  
  - **Risk:** Attackers can exploit the backdoor for remote root access or collect plaintext data
  - **Mitigation:** Disable anonymous login and upgrade to a secure alternative (SFTP)

- **22/tcp – SSH (OpenSSH 4.7p1)**  
  - Outdated version of OpenSSH, potential exposure to known vulnerabilities(Released in 2007)
  - Provides detailed version information (banner disclosure) to attackers  
  - Expands attack surface if not required
  - **Risk:** Brute-force password attacks or exploitation of known vulnerabilities
  - **Mitigation:** Restrict SSH access, enforce strong credentials, and upgrade to a modern release

- **23/tcp – Telnet (Linux telnetd)**  
  - Transmits all traffic, including usernames and passwords, in plaintext
  - Outdated protocol, replaced by SSH  
  - Accessible with default credentials in Metasploitable 2  
  - **Risk:** Enables credential theft through packet sniffing or unauthorized system access  
  - **Mitigation:** Disable Telnet entirely; use SSH for secure remote administration

- **25/tcp – SMTP (Postfix smtpd)**  
  - Reveals detailed version and system information in its banner (banner disclosure)  
  - Supports potentially dangerous commands like **VRFY/EXPN** (can allow attacker to enumerate valid users)  
  - May be configured as an **open relay**, allowing spammers or attackers to send unauthorized mail  
  - Uses **SSLv2**, an obsolete and insecure protocol
  - **Risk:** Attackers can gather system details, enumerate users, relay spam, or exploit weak encryption  
  - **Mitigation:** Disable unnecessary SMTP services, restrict relay permissions, upgrade to secure TLS versions, and harden Postfix configuration

- **53/tcp – DNS (BIND 9.4.2)**
  - Outdated version, zone transfer and cache poisoning risk
  - **Risk:** Internal network mapping, service compromise
  - **Mitigation:** Restrict queries, disable zone transfers, upgrade BIND

- **139/tcp & 445/tcp – Samba (3.0.20)**
  - Outdated SMB service, file access and remote code execution possible
  - **Risk:** Data exposure, privilege escalation
  - **Mitigation:** Upgrade Samba, restrict shares, enforce authentication

- **1524/tcp – Bind Shell**
  - Backdoor shell listening for connections
  - **Risk:** Immediate root access
  - **Mitigation:** Remove unnecessary backdoor services

- **3306/tcp – MySQL 5.0.51a**
  - Default credentials, outdated version
  - **Risk:** Unauthorized database access, potential code execution
  - **Mitigation:** Secure configuration, strong passwords, restrict access

- **5432/tcp – PostgreSQL 8.3.x**
  - Weak defaults, outdated version
  - **Risk:** Unauthorized database access
  - **Mitigation:** Upgrade, enforce authentication, restrict access

- **8180/tcp – Apache Tomcat/Coyote 1.1**
  - Outdated Tomcat, default apps, weak credentials
  - **Risk:** Remote code execution, sensitive file access
  - **Mitigation:** Upgrade Tomcat, remove default apps, secure credentials

## Analysis
- Almost all open ports on Metasploitable 2 are either **directly vulnerable** or **insecurely configured**, which is expected in a purpose-built lab environment.
- Critical attack surfaces include **services transmitting plaintext credentials, outdated software with known exploits, and exposed administrative or backdoor access**.
- This analysis highlights the importance of **patching, service hardening, and secure configuration practices** in production environments.

