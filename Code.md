# SkyTower Walkthrough

## Objective
Gain access to the `flag.txt` file inside the `/root/directory`

---

## Identifying the Target System

- Using `netdiscover` and obtain the IP
  - **Command:** `sudo netdiscover -P -r <IP>`
- Ping the device to see if it responds

---

## Step 1: Perform a Scan for Active Ports on the Device

**Command:**
```bash
nmap -A -T4 -p- <Target_IP>
```

### Findings

- We identify ports 22, 80, and 3128 are open on the device
- Port 22 has SSH enabled and is filtered
- Port 80 is running an Apache server
- Port 3128 is running a proxy server

---

## Step 2: Access the Server on Port 80 Through Browser

- Perform an SQL injection to obtain access to the server
- Use `-` in the uname and password fields to gain access

### Findings

- We have now accessed the server through the user `John` and have the account login details for the SkyTech server
- We can access John's account on the SkyTech server through SSH
- However, the nmap scan revealed Port 22 had SSH enabled; however, the SSH is filtered and hence we will not have direct access to SSH as a particular user
- Additionally, Port 3128 is running an HTTP proxy; we can now connect through SSH using the HTTP proxy running on the port
- To do this we use the `proxychains4` service

---

## Step 3: Configuring and Establishing Proxychains4

- We are attempting to SSH into the HTTP proxy hosted on Port 3128 with proxychains4
- Configuring proxychains4 to specify the use of this service prior to establishing the SSH connection
- Configuration can be done through the `/etc/proxychains4.conf` file
- Specify the type of proxychain and the target machine's IP and port
- Enable the Tor service through the command `service tor start` and `service tor status`
- The config file for the service must be updated with the target system's IP and port number that is running the proxy server

### Note: Using the Tor Service Through Proxychains4

Proxychains4 is a powerful Linux utility that forces network traffic from any TCP application through a chain of proxy servers (SOCKS4, SOCKS5, HTTP) for enhanced anonymity and to bypass network restrictions, routing traffic through multiple proxies like Tor or others to hide your real IP address and obfuscate activity.

It works by intercepting TCP calls and redirects them, allowing tools without native proxy support (like nmap, telnet) to use proxies, configurable via `/etc/proxychains4.conf` to define proxy lists and chain modes (strict, dynamic, random) for tasks in penetration testing or general privacy.

---

## Step 4: Connecting to the Server via SSH Through Proxychains4

- John's account details can be used to gain access to the server
- Spawn a bash shell once the connection is established (`/bin/bash`)

**Command:**
```bash
proxychains4 ssh john@<Target_IP> /bin/bash
```

### Findings

- We have now established partial access to the file system as John is not a privileged user
- John's account does not have any access to using `sudo` commands (`sudo -l`)
- The `root` directory is not accessible as it requires admin privileges; hence we must look for another user that could have the required access permissions

---

## Step 5: Navigating the File System to Find Additional User Details

- As John, we are limited to only a handful of files and directories that do not require admin privileges; hence we must find a different user that we could exploit
- The home directory displays a list of 3 users in total: John, Sara, and William
- We are unable to access the files for Sara and William while logged in as John

### Finding Account Details

- This brings up the need to find account details for either Sara or William
- From John's login page earlier, the email format for the users on the SkyTech server was `username@skytech`
- We can try accessing the user's details by using the email `sara@skytech.com`
- Through SQL injection techniques we can gain access to Sara's account and obtain the password
  - **Method:** `sara@skytech.com'#` (into the username field)

---

## Step 6: Logging Into the HTTP Proxy Through Sara's Credentials

**Command:**
```bash
proxychains4 ssh sara@<Target_IP> /bin/bash
```

### Findings

- Sara's account has higher access privileges than John's
- While Sara's account does not have complete `sudo` privileges, we identify that her account can access certain files through a specified format
- This is established through the `sudo -l` command that specifies Sara can access certain files as the root user through passing commands with the syntax:

**Command:**
```bash
sudo ls /accounts/../root
```

- The `flag.txt` file is located within the root directory; to access the file we use the command:

**Command:**
```bash
sudo cat /accounts/../root/directory/flags.txt
```
