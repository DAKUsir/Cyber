# Cyber
🔐 OpenSSL File Encryption Demo (AES-256-CBC)This document demonstrates the execution of a simple symmetric encryption and decryption process using the openssl command-line utility on Kali Linux.The goal is to encrypt a file (note.txt) using the AES-256-CBC cipher with a password derived using PBKDF2, and then decrypt it to recover the original message.PrerequisitesKali Linux operating system.openssl package installed (standard on Kali).Command Execution WalkthroughThe following steps and output blocks reflect the process of creating the plaintext file, encrypting it, and then successfully decrypting the ciphertext.Step 1: Create the Plaintext FileA file named note.txt is created with the secret message.┌──(chauhan㉿Chauhan)-[~]
└─$ echo "Top Secret: The flag is 'K4l1_R0x_47'" > note.txt

┌──(chauhan㉿Chauhan)-[~]
└─$ cat note.txt
Top Secret: The flag is 'K4l1_R0x_47'
Step 2: Encrypt the File (Plaintext to Ciphertext)The openssl enc command is used to encrypt note.txt into note.enc. You are prompted to enter a password, which is used to generate the encryption key. The -salt and -pbkdf2 flags ensure strong key derivation.┌──(chauhan㉿Chauhan)-[~]
└─$ openssl enc -aes-256-cbc -salt -pbkdf2 -in note.txt -out note.enc
enter AES-256-CBC encryption password: 
Verifying - enter AES-256-CBC encryption password:

┌──(chauhan㉿Chauhan)-[~]
└─$ cat note.enc
Salted__lIkr4$Kک\^_7-4xX# 1LNW[ѵ
Note: The output of cat note.enc starts with Salted__, followed by random bytes. This confirms the file is successfully encrypted and includes the encryption salt.Step 3: Decrypt the File (Ciphertext to Plaintext)The same command is used again, but with the critical -d flag to specify decryption. The ciphertext (note.enc) is read and written to the new plaintext file (recovered.txt).┌──(chauhan㉿Chauhan)-[~]
└─$ openssl enc -d -aes-256-cbc -salt -pbkdf2 -in note.enc -out recovered.txt 
enter AES-256-CBC decryption password:

# Verification of the decrypted content:
┌──(chauhan㉿Chauhan)-[~]
└─$ cat recovered.txt
Top Secret: The flag is 'K4l1_R0x_47'
The original message is recovered successfully, demonstrating the reversibility of the symmetric encryption process with the correct password.
