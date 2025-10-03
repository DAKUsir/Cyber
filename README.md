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
