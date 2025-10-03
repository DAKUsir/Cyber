🔐 OpenSSL File Encryption Demo (AES-256-CBC)

This document demonstrates the execution of a simple symmetric encryption and decryption process using the openssl command-line utility on Kali Linux.

The goal is to encrypt a file (note.txt) using the powerful AES-256-CBC cipher and then decrypt it to recover the original message.
Prerequisites

    Kali Linux operating system.

    openssl package installed (standard on Kali).

1. Ready-to-Run Commands (Copy & Paste)

Use these blocks to quickly execute the demo in your terminal. You will be prompted for a password for encryption and decryption.
1.1. Create Plaintext

echo "Top Secret: The flag is 'K4l1_R0x_47'" > note.txt
cat note.txt

1.2. Encrypt File

# Encrypts note.txt into note.enc (will prompt for password)
openssl enc -aes-256-cbc -salt -pbkdf2 -in note.txt -out note.enc
cat note.enc

1.3. Decrypt File

# Decrypts note.enc into recovered.txt (will prompt for the same password)
openssl enc -d -aes-256-cbc -salt -pbkdf2 -in note.enc -out recovered.txt
cat recovered.txt

2. Command Execution Walkthrough

This section provides a detailed trace of the commands and the expected output you experienced, demonstrating how the data changes state.
Step 1: Create the Plaintext File

A file named note.txt is created with the secret message and verified.

$echo "Top Secret: The flag is 'K4l1_R0x_47'" > note.txt$ cat note.txt
Top Secret: The flag is 'K4l1_R0x_47'

Step 2: Encrypt the File (Plaintext to Ciphertext)

The openssl enc command converts the readable note.txt into the unreadable ciphertext, note.enc.

    -aes-256-cbc: Sets the symmetric cipher for strong encryption.

    -salt & -pbkdf2: These flags ensure a robust, salted key derivation, making the password-based encryption much more secure against brute-force attacks.

$ openssl enc -aes-256-cbc -salt -pbkdf2 -in note.txt -out note.enc
enter AES-256-CBC encryption password: 
Verifying - enter AES-256-CBC encryption password:

$ cat note.enc
Salted__lIkr4$Kک\^_7-4xX# 1LNW[ѵ

Observation: The output of cat note.enc begins with Salted__, followed by non-printable, obfuscated data, confirming successful encryption.
Step 3: Decrypt the File (Ciphertext to Plaintext)

The critical -d flag is added to reverse the process. The same cipher, salt, and PBKDF2 function are used to regenerate the key required for decryption.

$ openssl enc -d -aes-256-cbc -salt -pbkdf2 -in note.enc -out recovered.txt 
enter AES-256-CBC decryption password:

$ cat recovered.txt
Top Secret: The flag is 'K4l1_R0x_47'

Result: The original message is recovered successfully, demonstrating the core principle of symmetric encryption.
