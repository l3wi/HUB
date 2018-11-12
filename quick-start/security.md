## Quick Start
### Security

Hub should run in a closed network protected from malicious actors

Hub has audit logs containing transaction data

For security purposes, Iota deposit addresses may only be used once.  For transactions processed by Hub, this is controlled internally. 

Funds should be safeguarded in hot wallets or cold storage.  Hub conducts “sweeps” periodically to transfer funds from a user deposit address to a hot wallet for safekeeping.    

#### Hub and Signing Server
To help prevent theft, Hub offers two servers:  hub and signing server.  For maximum security, run the signing server in a remote location because it stores the UUID and salt used in the hashing algorithms that secure seeds and deposit addresses.

UUID stands for Universally Unique IDentifier (UUID).  UUID is a 128-bit number used to identify information.

Hash algorithms are one way functions. They turn data into a fixed-length "hash" that cannot be reversed.  Changing the input of a hash algorithm changes the hash.  This is great for protecting passwords and deposit addresses because it helps guard against dictionary attacks and brute-force attacks.

The simplest way to crack a hash is to guess the string being hashed.  For example, an attacker can conduct a brute-force attack by testing every word in a dictionary.  Rainbow tables were devised to speed up this guessing game.  Attackers derive rainbow tables by pre-hashing dictionary words.

When hashing a password or deposit address, random data or "salt" can be added so the hash doesn’t match the rainbow table.  This makes it much more difficult for attackers to guess secret information.

IOTA signing-server stores salt and UUID in a remote location.  Thus, if the hub is compromised, the attacker will still lack sufficient information to break the deposit address codes and steal iota. 

The signing server is optional.  Salt can also be added to the Hub via the command line.  Salt must be greater than 20 characters long.
