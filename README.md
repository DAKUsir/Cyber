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
