### 23/tcp – Telnet

- **Vulnerability:** Telnet transmits credentials and session data in **plaintext**, making it vulnerable to interception and credential theft.  

- **Exploitation Steps:**
  1. On the client (Ubuntu VM), connected to the Telnet server (Metasploitable2):  
     ```bash
     telnet 192.168.56.102
     ```
  2. Entered username and password at the login prompt.  
  3. On Kali (attacker/sniffer), captured traffic with tcpdump:  
     ```bash
     sudo tcpdump -i eth0 port 23 -w telnet_capture.pcap
     ```
  4. Opened the capture in Wireshark and filtered for Telnet traffic:  
     ```
     tcp.port == 23
     ```
  5. Followed the TCP stream and recovered plaintext username and password.

- **Result:** Successfully intercepted and extracted valid credentials from Telnet traffic. Demonstrated that attackers can gain unauthorized access by sniffing the network.  

- **Risk:** Credentials and session activity are fully exposed to network sniffing, allowing attackers to hijack accounts and escalate access.  

- **Mitigation:**  
  - Disable Telnet and remove the service from the system.  
  - Block port 23 via firewall (`ufw deny 23/tcp`).  
  - Replace Telnet with **SSH** for encrypted remote access.  
  - Enable additional protections such as **fail2ban** and a host firewall to reduce brute force and unauthorized login attempts.