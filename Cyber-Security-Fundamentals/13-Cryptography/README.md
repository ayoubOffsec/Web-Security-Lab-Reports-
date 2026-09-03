# Cryptography

## Introduction

The Cryptography lab series from TryHackMe provides a comprehensive and structured learning path covering the fundamentals of cryptography, from theoretical concepts to practical applications and tools. This series consists of four labs:

    Cryptography Basics

    Public Key Cryptography Basics

    Hashing Basics

    John the Ripper: The Basics

This knowledge forms an essential foundation for anyone in the cybersecurity field, as cryptography is used everywhere: from securing passwords, to protecting communications, to verifying file integrity.
Lab 1: Cryptography Basics
Lab Content and Objectives

This lab introduces the fundamental concepts of cryptography, including key terminology, symmetric algorithms, and basic mathematical principles used in encryption.
Concepts Covered in This Lab
1. Basic Cryptography Terminology
Term	Definition
Plaintext	The original readable message or data before encryption
Ciphertext	The scrambled unreadable version of the message after encryption
Cipher	The algorithm or method used to convert plaintext to ciphertext and vice versa
Key	A string of bits used by the cipher algorithm for encryption or decryption
Encryption	The process of converting plaintext to ciphertext
Decryption	The reverse process, converting ciphertext back to plaintext

Important Note: The algorithm is usually public knowledge, but the key must remain secret (except in asymmetric cryptography where the public key is openly shared).
2. Caesar Cipher

    One of the oldest encryption algorithms

    Shifts letters by a fixed number of positions

    Example: Shift of 3: TRYHACKME → WUBKDFNPH

3. Symmetric Encryption

Characteristics:

    Uses a single key for both encryption and decryption

    Fast and efficient for large amounts of data

    Challenge: Needs a secure channel for key sharing

Common Symmetric Algorithms:
Algorithm	Adoption Year	Key Size	Status
DES	1977	56 bits	Insecure (broken in 1999)
3DES	-	168 bits	Legacy, not recommended
AES	2001	128, 192, 256 bits	Secure and widely used
4. XOR and Modulo Operations

    XOR is a fundamental operation in cryptography: A ⊕ A = 0 and A ⊕ 0 = A

    Modulo is the remainder operation, used in many algorithms like RSA

5. Importance of Cryptography in Practice

    Protecting credit card data (PCI DSS standard)

    Securing website logins

    Protecting sensitive data in storage and transit

Lab 2: Public Key Cryptography Basics
Lab Content and Objectives

This lab covers asymmetric cryptography (public key cryptography), including how public and private keys work, the RSA algorithm, Diffie-Hellman key exchange, and applications like SSH and PGP.
Concepts Covered in This Lab
1. What is Asymmetric Cryptography?

Definition: A method of securing data using a pair of keys: a public key and a private key. It enables secure communication without the need to share secret keys in advance.
Property	Description
Public Key	Shared with everyone, used for encryption
Private Key	Kept secret, used for decryption
Direction	What is encrypted with the public key can only be decrypted with the private key
2. Security Properties Provided by Asymmetric Cryptography
Property	Description
Authentication	Verifying the identity of the other party
Authenticity	Ensuring the message came from the claimed sender
Integrity	Ensuring data was not modified during transmission
Confidentiality	Preventing unauthorized parties from reading data
3. Common Usage Pattern: Key Exchange

Asymmetric encryption is slow compared to symmetric encryption, so it is typically used to securely exchange a symmetric key, which is then used for fast session encryption.

Analogy:

    Public Key = Lock

    Private Key = Key to the lock

    Symmetric Key = Secret combination

The secret combination is sent in a box locked with the public key; only the owner of the private key can open it.
4. RSA Algorithm

Security Foundation: The difficulty of factoring large numbers into their prime factors (it's easy to multiply two primes, but very hard to factor their product).

Mathematical Components:

    p, q = Two large prime numbers

    n = p × q (part of the key)

    φ(n) = (p-1) × (q-1) (Euler's totient function)

    Public key: (n, e)

    Private key: (n, d)

Use Cases: Encryption, digital signatures, key exchange.

Practical Note: Real keys use primes hundreds of digits long, making factorization practically impossible.
5. Diffie-Hellman Key Exchange

Purpose: Enables two parties to agree on a shared secret key over an insecure channel, without directly sending the key itself.

How It Works:

    Both parties exchange public values

    Each party combines their own private value with the other party's public value

    Due to mathematical properties, both parties arrive at the same shared secret

Weakness: Vulnerable to Man-in-the-Middle (MITM) attacks without authentication.
6. SSH and Public Key Applications

    SSH uses key pairs for secure authentication instead of passwords

    Generate key: ssh-keygen -t ed25519

    Copy key: ssh-copy-id user@host

    Security: Private key permissions should be 600

7. Digital Signatures and Certificates

    Digital Signature: Signed with the private key, verified with the public key; provides authenticity and integrity

    Certificates (X.509): Bind a public key to an identity through a chain of trust from Certificate Authorities (CA)

8. PGP / GPG

Uses: Encrypting, decrypting, and signing emails and files using a system of public and private keys.
Lab 3: Hashing Basics
Lab Content and Objectives

This lab covers hash functions, how they are used to secure passwords and verify file integrity, as well as weaknesses in older algorithms and the importance of salting.
Concepts Covered in This Lab
1. What is a Hash Function?

A mathematical function that converts any input data (of any size) into a fixed-length value (hash value).

Core Properties:

    One-Way Function: Cannot recover the original data from the hash

    Avalanche Effect: Any small change in input causes a large change in the hash

    Same Input = Same Hash

2. Common Hash Algorithms
Algorithm	Output Length	Security Status
MD5	32 hex characters (128 bits)	Insecure, broken
SHA-1	40 hex characters (160 bits)	Insecure, broken
SHA-256	64 hex characters (256 bits)	Secure, widely used
3. Why Hashing Matters
Use Case	Description
Password Storage	Store the hash instead of the plaintext password
File Integrity Verification	Compare hash values to check if a file was modified
Deduplication	Same data yields the same hash
4. Insecure Password Storage Methods
Method	Problem
Storing Plaintext	Anyone with database access can read all passwords
Encrypting Passwords	If the key is leaked, all passwords can be decrypted
Unsalted Hashing	Vulnerable to Rainbow Table attacks

Real-World Example: The LinkedIn breach used SHA-1 without salt, making password cracking easy.
5. Secure Password Storage

Best Practices:

    Use slow, hardware-resistant Key Derivation Functions (KDFs): Argon2, scrypt, bcrypt, PBKDF2

    Add a unique salt per user: Prevents rainbow table attacks and ensures identical passwords produce different hashes

    Store hash + salt + parameters: Never store the password itself

6. Identifying Hash Types
Type	Format	Example
MD5	32 hex characters	5d41402abc4b2a76b9719d911017c592
SHA-1	40 hex characters	a94a8fe5ccb19ba61c4c0873d391e987982fbbd3
SHA-256	64 hex characters	2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824
/etc/shadow (Linux)	Starts with $	$6$salt$hash where $6 = sha512crypt
7. Collisions

Definition: A situation where two different inputs produce the same hash value (due to the Pigeonhole Principle).

Risk: MD5 and SHA-1 have known collisions and should not be used for security purposes.
8. HMAC (Keyed-Hash Message Authentication Code)

Combines a hash function with a secret key to provide:

    Integrity: Ensures the message was not altered

    Authenticity: Ensures the sender possesses the secret key

Common Uses: API request signing, message integrity verification.
Lab 4: John the Ripper: The Basics
Lab Content and Objectives

This lab introduces John the Ripper (JtR), the most popular password cracking tool. It covers dictionary attacks, wordlist modes, custom rules, and the use of conversion tools.
Concepts Covered in This Lab
1. What is John the Ripper?

A high-speed command-line tool for cracking hashes and passwords, supporting:

    Dictionary attacks

    Single crack mode

    Custom rules

    Cracking protected files (ZIP, RAR, SSH keys)

2. Basic Syntax
bash

john [options] [hash_file]

Basic Examples:
bash

# Basic dictionary attack
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt

# Specify hash type (faster and more accurate)
john --format=raw-md5 --wordlist=rockyou.txt hash.txt

# Show cracked hashes
john --show hash.txt

3. Identifying Hash Types

    Automatic Detection: john attempts to identify the type automatically

    Manual Specification: Use --format=<format> for faster and more accurate results

    List Formats: john --list=formats | grep -i md5

4. Main Operation Modes
Mode	Description	Command
Wordlist Mode	Test words from a list	john --wordlist=file.txt hash.txt
Single Crack Mode	Generate candidates from username and GECOS info	john --single hash.txt
Custom Rules	Apply transformations to words (e.g., add numbers and symbols)	john --wordlist=file.txt --rule=RuleName hash.txt
5. Attacking System Hashes

Linux (/etc/shadow):

    Combine /etc/passwd and /etc/shadow:
    bash

unshadow passwd_file shadow_file > unshadowed.txt

Crack the hash:
bash

john --wordlist=rockyou.txt --format=sha512crypt unshadowed.txt

Windows (NTLM):

    Hash used to store passwords in Windows

    Use the type --format=nt

6. Conversion Tools (2john)
Tool	Use Case	Command
zip2john	Extract hash from password-protected ZIP	zip2john file.zip > zip_hash.txt
rar2john	Extract hash from password-protected RAR	rar2john file.rar > rar_hash.txt
ssh2john	Extract hash from SSH private key	ssh2john id_rsa > id_rsa_hash.txt
7. Custom Rules

Purpose: Simulate common password patterns in organizations (e.g., word + number + symbol).

Configuration File: john.conf (usually found in /etc/john/ or /opt/john/)

Example Rule:
text

[List.Rules:PoloPassword]
c Az"[0-9][!£$%@]"

    c → Capitalize first letter

    Az → Append after the word

    [0-9][!£$%@] → Add a number then a symbol

Usage:
bash

john --wordlist=rockyou.txt --rule=PoloPassword hash.txt

8. Practical Tips
Tip	Description
Start with the right wordlist	rockyou.txt is powerful and sufficient in most cases
Use Single Mode	Very useful when you have usernames and additional info
Use Custom Rules	Effective when organizations enforce password complexity
Specify Hash Type	Makes cracking significantly faster and more accurate
Use 2john tools	Crack protected files, not just hashes
9. Command Summary
Command	Description
john hash.txt	Attempt cracking using automatic mode
john --wordlist=rockyou.txt hash.txt	Dictionary attack with specified wordlist
john --single hash.txt	Single crack mode
john --format=raw-md5 hash.txt	Specify hash type
john --show hash.txt	Show cracked hashes
john --list=formats	List all supported hash types
unshadow passwd shadow > out.txt	Prepare Linux hashes for cracking
zip2john file.zip > hash.txt	Extract hash from ZIP file
Conclusion

The Cryptography lab series from TryHackMe provides comprehensive knowledge in cryptography, covering:
Level	Topics
Basics	Cryptography terminology, symmetric encryption, Caesar cipher, XOR and modulo operations
Asymmetric Cryptography	RSA, Diffie-Hellman, SSH, digital signatures, PGP/GPG, certificates
Hashing	Hash functions, secure password storage, salting, HMAC, identifying hash types
Cracking Tools	John the Ripper, operation modes, custom rules, 2john tools

This knowledge forms an essential foundation for anyone in cybersecurity, as cryptography is used in securing passwords, protecting communications, and verifying data integrity.
