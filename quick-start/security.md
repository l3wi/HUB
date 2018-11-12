## Quick Start - Security

#### Environment
Hub should run in a closed network protected from malicious actors

#### Logs
Hub has audit logs containing transaction data

#### Deposit Addresses
For security purposes, deposit addresses may only be used once.  For transactions processed by IOTA Hub, this is controlled internally.   Hub conducts periodic “sweeps” to transfer funds from a user deposit address to a hot wallet to safeguard funds.      

#### Hub and Signing Server
To help prevent theft, IOTA Hub offers two servers:  hub and signing server.  Hub performs key functions for managing tasks associated with deposits and withdrawals of Iota.  The signing server stores key security data, UUID and salt.  These are used in the hashing algorithms that secure IOTA seeds and deposit addresses.  For maximum security, run the signing server in a remote location.  Therefore, if hub is compromised, malicious actors will lack sufficient data to easily decipher transactions and steal funds.  

UUID stands for Universally Unique IDentifier (UUID).  UUID is a 128-bit number used to identify information.  Salt is a random string that must be greater than 20 characters long.

The signing server is optional.  Salt can also be added to the Hub via the command line.  However, this configuration keeps all data in one location and does not provide maximum security.

**Guarding Against Dictionary Attacks and Brute-force attacks**

Hash algorithms are one way functions. They turn data into a fixed-length "hash" that cannot be reversed.  Changing the input of a hash algorithm changes the hash.  That is why salt is added to the computation.  It is great for protecting passwords and deposit addresses because it helps guard against dictionary attacks and brute-force attacks.

The simplest way to crack a hash is to guess the string being hashed.  For example, an attacker can conduct a brute-force attack by testing every word in a dictionary.  Rainbow tables were devised to speed up this guessing game.  Attackers derive rainbow tables by pre-hashing dictionary words.

When hashing a password or deposit address, random data or "salt" can be added so the hash doesn’t match the rainbow table.  This makes it much more difficult for attackers to guess secret information.

By running the signing-server in a remote location, an attacker will lack sufficient information to break the deposit address codes and steal iota. 
