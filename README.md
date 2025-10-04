# 🔐 OpenSSL File Encryption Demo (AES-256-CBC)

This document demonstrates the execution of a simple symmetric encryption and decryption process using the `openssl` command-line utility on Kali Linux.

The goal is to encrypt a file (`note.txt`) using the powerful AES-256-CBC cipher and then decrypt it to recover the original message.

### Prerequisites

- **Kali Linux** operating system.
- **openssl** package installed (standard on Kali).

---

### 1. Ready-to-Run Commands (Copy & Paste)

Use these blocks to quickly execute the demo in your terminal. You will be prompted for a password during both encryption and decryption.

#### 1.1. Create Plaintext

Create a file (`note.txt`) containing a secret message and verify its contents.

```bash
echo "Top Secret: The flag is 'K4l1_R0x_47'" > note.txt
cat note.txt
```

#### 1.2. Encrypt File

Encrypt `note.txt` into `note.enc` (you will be prompted to set a password).

```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -in note.txt -out note.enc
cat note.enc
```

#### 1.3. Decrypt File

Decrypt `note.enc` back into the original file (`recovered.txt`). Enter the same password used for encryption.

```bash
openssl enc -d -aes-256-cbc -salt -pbkdf2 -in note.enc -out recovered.txt
cat recovered.txt
```

---

### 2. Command Execution Walkthrough

This section provides a detailed trace of the commands and expected output, demonstrating how the data changes state.

#### Step 1: Create the Plaintext File

The file `note.txt` is created with the secret message and verified.

```bash
$ echo "Top Secret: The flag is 'K4l1_R0x_47'" > note.txt
$ cat note.txt
Top Secret: The flag is 'K4l1_R0x_47'
```

#### Step 2: Encrypt the File (Plaintext to Ciphertext)

The `openssl enc` command converts the readable `note.txt` into the unreadable ciphertext `note.enc`.

* `-aes-256-cbc`: Sets the symmetric cipher for strong encryption.
* `-salt & -pbkdf2`: These flags ensure a robust, salted key derivation, making the password-based encryption much more secure against brute-force attacks.

```bash
$ openssl enc -aes-256-cbc -salt -pbkdf2 -in note.txt -out note.enc
enter AES-256-CBC encryption password:
Verifying - enter AES-256-CBC encryption password:
```

The `cat note.enc` command will show an obfuscated result similar to the following (with binary data):

```bash
$ cat note.enc
Salted__lIkr4$Kک\^_7-4xX# 1LNW[ѵ
```

#### Step 3: Decrypt the File (Ciphertext to Plaintext)

The `-d` flag reverses the process. The same cipher, salt, and PBKDF2 function are used to regenerate the key required for decryption.

```bash
$ openssl enc -d -aes-256-cbc -salt -pbkdf2 -in note.enc -out recovered.txt
enter AES-256-CBC decryption password:
```

The decrypted content will be restored to its original form:

```bash
$ cat recovered.txt
Top Secret: The flag is 'K4l1_R0x_47'
```

---

### Result

The original message is successfully recovered, demonstrating the core principle of symmetric encryption.

# Firewall Demonstration using iptables in Kali Linux

This project demonstrates how to configure a simple firewall in **Kali Linux** using `iptables`.  
It includes flushing existing rules, setting default policies, allowing essential traffic, blocking IPs, and logging dropped packets — with real terminal outputs.

---

```bash
┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers

[sudo] password for chauhan: 
Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
                                                                         
┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X
sudo iptables -t mangle -F
sudo iptables -t mangle -X

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
                                                                         
┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers

Chain INPUT (policy DROP 107 packets, 9770 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy DROP 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 54 packets, 21267 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
                                                                         
┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A INPUT -i lo -j ACCEPT

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers
Chain INPUT (policy DROP 339 packets, 34368 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           

Chain FORWARD (policy DROP 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 163 packets, 35995 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
                                                                         
┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -j ACCEPT

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A INPUT -p tcp --dport 80 -m conntrack --ctstate NEW -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -m conntrack --ctstate NEW -j ACCEPT

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers
Chain INPUT (policy DROP 809 packets, 86390 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
2        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:22 ctstate NEW
3        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:80 ctstate NEW
4        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:443 ctstate NEW

Chain FORWARD (policy DROP 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 589 packets, 67987 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
                                                                         
┌──(chauhan㉿Chauhan)-[~]
└─$ ssh localhost
ssh: connect to host localhost port 22: Connection refused

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -I INPUT 1 -s 198.51.100.23 -j DROP

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers            
Chain INPUT (policy DROP 1008 packets, 111K bytes)
num   pkts bytes target     prot opt in     out     source               destination         
1        0     0 DROP       all  --  *      *       198.51.100.23        0.0.0.0/0           
2        2   100 ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0           
3        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:22 ctstate NEW
4        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:80 ctstate NEW
5        0     0 ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:443 ctstate NEW

Chain FORWARD (policy DROP 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 780 packets, 82601 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -N LOGDROP             

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A LOGDROP -m limit --limit 5/min -j LOG --log-prefix "IPTables-Dropped:" --log-level 4

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A LOGDROP -j DROP

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -A INPUT -j LOGDROP

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo journalctl -k -f            
Oct 04 01:32:01 Chauhan kernel: RPC: Registered udp transport module.
Oct 04 01:32:01 Chauhan kernel: RPC: Registered tcp transport module.
Oct 04 01:34:00 Chauhan kernel: UDF-fs: warning (device sr0): udf_load_vrs: No VRS found                                                                  
Oct 04 01:34:00 Chauhan kernel: ISO 9660 Extensions: Microsoft Joliet Level 3
Oct 04 01:34:00 Chauhan kernel: ISO 9660 Extensions: RRIP_1991A
^C

┌──(chauhan㉿Chauhan)-[~]
└─$ curl -l https://www.google.com
curl: (6) Could not resolve host: www.google.com

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -F                     
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t mangle -F
sudo iptables -P INPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
sudo iptables -P OUTPUT ACCEPT

┌──(chauhan㉿Chauhan)-[~]
└─$ sudo iptables -L -v -n --line-numbers
Chain INPUT (policy ACCEPT 17 packets, 1900 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
num   pkts bytes target     prot opt in     out     source               destination         

Chain OUTPUT (policy ACCEPT 18 packets, 2562 bytes)
num   pkts bytes target     prot opt in     out     source               destination         
```
# Steganography Demo with Steghide (Kali Linux)

This is a simple demonstration of using **steghide** to hide and extract secret data inside an image file.

---

## Steps

### 1. Verify Steghide installation
```bash
sudo apt install steghide -y
```
Output:
```
steghide is already the newest version (0.5.1-15).
```

### 2. Create a secret text file
```bash
echo "This is hidden secret" > secret.txt
```

### 3. Embed the secret into an image
```bash
steghide embed -cf com.jpeg -ef secret.txt
```
Output:
```
Enter passphrase: 
Re-Enter passphrase: 
embedding "secret.txt" in "com.jpeg"... done
```

### 4. Check information about the image
```bash
steghide info com.jpeg
```
Output:
```
"com.jpeg":
  format: jpeg
  capacity: 289.0 Byte
Try to get information about embedded data ? (y/n) y
Enter passphrase: 
  embedded file "secret.txt":
    size: 22.0 Byte
    encrypted: rijndael-128, cbc
    compressed: yes
```

### 5. Extract the hidden secret
```bash
steghide extract -sf com.jpeg
```
Output:
```
Enter passphrase: 
the file "secret.txt" does already exist. overwrite ? (y/n) y
wrote extracted data to "secret.txt".
```

### 6. Verify the extracted secret
```bash
cat secret.txt
```
Output:
```
This is hidden secret
```

---

## Summary
- **secret.txt** was successfully hidden inside **com.jpeg** using `steghide`.
- The file was later extracted with the same passphrase, proving the concept of steganography.

# Zphisher Setup and Usage Guide

Zphisher is an open-source phishing toolkit for educational simulations, offering over 30 templates for fake login pages (e.g., Facebook, Instagram, Google). This guide provides steps to set up and run Zphisher on Kali Linux for **ethical, authorized** security training only. **Unauthorized phishing is illegal** under laws like the Computer Fraud and Abuse Act. Always obtain written consent and use isolated networks.

## Prerequisites
- Kali Linux (2025.x recommended).
- Internet connection for tunneling (e.g., Ngrok).
- Root privileges for dependency installation.
- Basic tools: Git, Python 3, PHP, curl.

## Installation Steps
1. **Update Kali Linux**:
   Update your system to ensure compatibility:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Dependencies**:
   Install required tools (Zphisher auto-installs others):
   ```bash
   sudo apt install git curl php openssh-server -y
   ```

3. **Clone Zphisher Repository**:
   Download Zphisher from GitHub:
   ```bash
   git clone https://github.com/htr-tech/zphisher.git
   cd zphisher
   ```

4. **Set Up Ngrok for Tunneling**:
   - Download and install Ngrok:
     ```bash
     wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-v3-stable-linux-amd64.tgz
     tar -xvzf ngrok-v3-stable-linux-amd64.tgz
     sudo mv ngrok /usr/local/bin/
     ```
   - Sign up at [ngrok.com](https://ngrok.com) for an authtoken, then authenticate:
     ```bash
     ngrok authtoken YOUR_AUTHTOKEN
     ```

5. **Run Zphisher**:
   - Start the script (dependencies install automatically on first run):
     ```bash
     bash zphisher.sh
     ```
   - Select a phishing category (e.g., `01` for social media).
   - Choose a template (e.g., `01` for Facebook).
   - Select tunneling (e.g., `1` for Ngrok).
   - Accept the default port (3333) or specify a custom one.
   - The tool starts a PHP server and generates public URLs.

6. **Simulate Phishing**:
   - Share the generated URL (e.g., `https://compliant-poor-findarticles-yields.trycloudflare.com`) with a consenting test user.
   - The user visits the fake page and enters credentials.
   - Captured data (IP, username, password) is logged in the terminal and saved to `auth/ip.txt` and `auth/usernames.dat`.

7. **Stop the Attack**:
   - Press `Ctrl+C` to stop the server and Ngrok.
   - Check `auth/` directory for logs.

## Expected Output
Below is an example of Zphisher's terminal output during a simulation:

```
  ░▀▀█░█▀█░█░█░▀█▀░█▀▀░█░█░█▀▀░█▀▄
  ░▄▀░░█▀▀░█▀█░░█░░▀▀█░█▀█░█▀▀░█▀▄
  ░▀▀▀░▀░░░▀░▀░▀▀▀░▀▀▀░▀░▀░▀▀▀░▀░▀ 2.3.5

[-] URL 1 : https://compliant-poor-findarticles-yields.trycloudflare.com

[-] URL 2 : https://

[-] URL 3 : https://facabook.com@

[-] Waiting for Login Info, Ctrl + C to exit...

[-] Victim IP Found !
[-] Victim's IP : 152.57.129.105
[-] Saved in : auth/ip.txt

[-] Victim IP Found !
 152.57.129.105 : 152.57.129.105
[-] Saved in : auth/ip.txt

[-] Login info Found !!
[-] Account : dfdd
[-] Password : d
[-] Saved in : auth/usernames.dat

[-] Waiting for Next Login Info, Ctrl + C to exit.

[-] Victim IP Found !
[-] Victim's IP : 152.57.129.105
[-] Saved in : auth/ip.txt
^C
[!] Program Interrupted.
```

## Ethical and Legal Considerations
- **Authorized Use Only**: Conduct simulations with explicit permission in controlled environments (e.g., cybersecurity labs).
- **Legal Risks**: Unauthorized phishing violates laws and can lead to prosecution.
- **Prevention Tips**:
  - Verify URLs (check HTTPS, avoid misspellings).
  - Use multi-factor authentication (MFA).
  - Report phishing to services like [PhishTank](https://phishtank.org).

## Troubleshooting
- **Dependency Errors**: Manually install missing tools (`sudo apt install wget php-cgi`).
- **Ngrok Fails**: Verify authtoken; try Serveo (`2` in menu) as an alternative.
- **Port Conflicts**: Change port in Zphisher menu or kill processes (`sudo fuser -k 3333/tcp`).

## Alternatives
- **Gophish**: For structured phishing campaign simulations.
- **SET (Social-Engineer Toolkit)**: Advanced phishing and social engineering.

## Disclaimer
Zphisher is for **educational purposes only**. The author and this guide are not responsible for misuse. Always adhere to ethical hacking guidelines.


# PentBox Honeypot Setup on Kali Linux

## Overview
This guide demonstrates how to set up a simple honeypot using **PentBox**, a Ruby-based penetration testing tool. The honeypot acts as a decoy service (default on port 80/HTTP) to detect and log unauthorized access attempts. It's low-interaction, logging IP addresses, ports, and requests without providing real access.

**Purpose**: Monitor intruder tactics for defensive analysis. Suitable for educational or testing environments.

**Requirements**:
- Kali Linux (tested on 2025.3 or similar).
- Root access for port binding.
- Isolated network/VM (e.g., VirtualBox with NAT) to avoid public exposure.

## Installation

1. **Update Kali and Install Dependencies**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install ruby-full git -y
   ```

2. **Clone and Extract PentBox**:
   The repository contains a compressed archive; extract it to access the tool.
   ```bash
   cd ~
   rm -rf pentbox  # Remove if exists
   git clone https://github.com/technicaldada/pentbox
   cd pentbox
   tar -zxvf pentbox.tar.gz
   cd pentbox
   chmod +x pentbox.rb
   ```

## Usage

1. **Run PentBox**:
   ```bash
   sudo ./pentbox.rb
   ```

2. **Configure the Honeypot**:
   - In the menu, select `2` (Network Tools) → Enter.
   - Select `3` (Honeypot) → Enter.
   - For quick setup: Select `1` (Fast Auto Configuration) → Enter. Activates on port 80 with default log at `other/log_honeypot.txt`.
   - For custom: Select `2` (Manual Configuration) → Enter, then input port (e.g., 80), warning message (e.g., "Access Denied!"), log path, and beep option (y/n).
   - Output: "HONEYPOT ACTIVATED ON PORT 80". Keep the terminal open for monitoring.

3. **Allow Port Access**:
   Ensure the port is open and no conflicts (e.g., stop Apache if running).
   ```bash
   sudo systemctl stop apache2
   sudo ufw allow 80/tcp
   ```

## Testing

1. **Find Kali IP**:
   ```bash
   ip addr show  # Note IP (e.g., 192.168.x.x under eth0/wlan0)
   ```

2. **Simulate Attack from Another Device**:
   - On a separate VM/machine: Open browser to `http://<kali_IP>` or run:
     ```bash
     nc <kali_IP> 80
     ```
   - Expected: Warning message displayed on the tester; alert on Kali terminal: "INTRUSION ATTEMPT DETECTED! from <IP>:<port>" with request details (e.g., GET / HTTP/1.1).

## Viewing Logs

Logs record timestamps, IPs, ports, and full requests.
```bash
cat other/log_honeypot.txt
```

## Troubleshooting

- **Command Not Found**: Ensure extraction (`tar -zxvf`) and `chmod +x pentbox.rb`. Run with `sudo ruby pentbox.rb` if needed.
- **Port Conflict**: Stop services (`sudo systemctl stop apache2`) or use another port (e.g., 8080) in manual config.
- **No Alerts**: Test from a different IP (not localhost). Create `other/` dir if missing: `mkdir -p other`.
- **Firewall**: Check status: `sudo ufw status`. Allow port if blocked.
- **Alternative Download**: If GitHub fails, use SourceForge: `wget https://sourceforge.net/projects/pentbox/files/pentbox-1.8.tar.gz`, then `tar -xvzf pentbox-1.8.tar.gz && cd pentbox-1.8`.

## Notes
- **Security**: Use in isolated setups only. Honeypots can attract real attacks.
- **Limitations**: Low-interaction; for advanced, try Cowrie (`sudo apt install cowrie`).
- **Persistence**: Run in background: `screen sudo ./pentbox.rb` (detach: Ctrl+A, D; reattach: `screen -r`).
- **License**: PentBox is open-source; check repo for details.

For questions, refer to the PentBox GitHub repo.

#Top 10 Essential Nmap Commands (Bash & Usage)This list contains the 10 most crucial Nmap commands for quick reference and effective network reconnaissance.1. Default Scannmap target-ip
Use: Performs a standard scan on the top 1,000 most popular TCP ports. This is the simplest and most common starting point for initial target discovery.2. Aggressive Scansudo nmap -A target-ip
Use: Enables OS detection, service version detection (-sV), default script scan (-sC), and traceroute in one comprehensive, high-detail, but often noisy scan. Requires sudo.3. Stealth Scan (SYN)sudo nmap -sS target-ip
Use: Executes the TCP SYN "half-open" scan. It is the fastest and most popular scan technique, as it avoids completing the TCP handshake. Requires sudo.4. Scan All Portsnmap -p- target-ip
Use: Scans all 65,535 TCP ports. Use this when you need absolute confirmation of all open ports, though it is significantly slower than the default scan.5. Specify Custom Portsnmap -p 22,80,443 target-ip
Use: Focuses the scan exclusively on a custom list or range of ports. This greatly speeds up the scan when you are looking for specific services.6. Service Version Detectionnmap -sV target-ip
Use: Identifies the precise application and version number of services running on the discovered open ports (e.g., Apache 2.4.41). Crucial for vulnerability assessment.7. Default Script Scannmap -sC target-ip
Use: Runs a set of safe, pre-written Nmap Scripting Engine (NSE) scripts to gather deeper information, such as web server titles, or to check for anonymous login access.8. Ping Scan Onlynmap -sn 192.168.1.0/24
Use: Disables port scanning and only checks to see which hosts in a specified network range are currently alive. This is used for quick "host sweep" network mapping.9. No Ping Scan (Force Scan)nmap -Pn target-ip
Use: Skips the initial host discovery check. This is necessary when a target is protected by a firewall that blocks ICMP (ping) traffic, forcing Nmap to assume the host is online and proceed with port scanning.10. Output All Formatsnmap -oA results target-ip
Use: Saves the scan results to three files (results.nmap, results.xml, results.gnmap). This is essential for detailed reporting, documentation, and importing data into other analysis tools.
