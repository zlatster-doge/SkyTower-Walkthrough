### SkyTower Penetration Testing Lab (STAR Method)

**Situation:**  
Assigned a controlled penetration-testing lab (SkyTower) with the objective of obtaining a protected `flag.txt` file from the `/root` directory on an unknown Linux system.

**Task:**  
Identify exposed services, exploit application and network weaknesses, escalate access across multiple user accounts, and bypass access controls to retrieve sensitive data.

**Action:**  
- Conducted network discovery and full port enumeration using `netdiscover` and `nmap`, identifying exposed SSH, Apache web services, and an HTTP proxy.  
- Exploited a SQL injection vulnerability on the web application to obtain valid user credentials.  
- Bypassed filtered SSH restrictions by configuring and leveraging `proxychains4` with an HTTP proxy and Tor to establish remote shell access.  
- Performed privilege analysis (`sudo -l`) and enumerated user accounts to identify escalation paths.  
- Exploited misconfigured sudo permissions using directory traversal techniques to access restricted root files.

**Result:**  
Successfully escalated privileges from a low-level user to a higher-privileged account and retrieved the protected `flag.txt` file from the root directory, demonstrating effective exploitation of web vulnerabilities, proxy-based network evasion, and privilege misconfiguration.
