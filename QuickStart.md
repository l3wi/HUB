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

### Installation

Installation was tested using Ubuntu 18.04 LTS **Server** running on Oracle VirtualBox.

Note:  There are known errors attempting to install this on Ubuntu 18.04 LTS **Desktop**

#### Step 1: Installing dependancies

First update the server

```sudo apt update```

Install a compiler

```sudo apt install gcc-7```

Install dependences for Bazel, the development tool used to create IOTA Hub

```sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python```

Download the binary installer for the latest version of Bazel

```wget https://github.com/bazelbuild/bazel/releases/download/0.18.0/bazel-0.18.0-installer-linux-x86_64.sh```

Change permissions to allow the script to execute

```chmod +x bazel-0.18.0-installer-linux-x86_64.sh```

Install under the currently active user 

```./bazel-0.18.0-installer-linux-x86_64.sh --user```

Install the final dependency, pyparsing

```sudo apt-get install python-pyparsing```

#### Step 2: Installing the database

IOTA Hub uses MariaDB 10.2.1+ to store users, addresses, balances, etc.
MariaDB is not pre-loaded on Ubuntu 18.04 LTS Server.  To install, first add a GPG key for the MariaDB repository

```sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8```

Then add the MariaDB repository with the following command

```sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://ftp.utexas.edu/mariadb/repo/10.3/ubuntu bionic main'```

Update the package list

```sudo apt update```

Install the MariaDB Server

```sudo apt install mariadb-server```

During the installation you will be prompted to enter a root password for MariaDB

Check that the database version is higher than the minimum required of 10.2.1

```mysql --version```

#### Step 3: Building the Hub

Clone the source code from GitHub repository

```git clone https://github.com/iotaledger/rpchub.git hub```

Go to the the newly created hub directory

```cd hub```

Build the Hub from source

```bazel build -c opt //hub:hub```

Check that the build completed successfully

#### Step 4: Setting up the database

Create the hub database and add the schema and triggers

```echo "CREATE DATABASE hub" | mysql -uroot -pmyrootpassword```

Load the provided schema from the Hub source into the hub database

```mysql -h127.0.0.1 -uroot -pmyrootpassword hub < schema/schema.sql```

Import the database triggers

```mysql -h127.0.0.1 -uroot -pmyrootpassword hub < schema/triggers.mariadb.sql```

#### Step 5: Running the Hub

Hub has quite a few settings.  For easier launch, these can be stored in a shell script. 

```nano start.sh```

First, set the "salt" parameter. Salt is used to generate more secure seeds.  Make sure the salt strings is longer than 20 characters.  Do not share your salt.
Next, set the database name to "hub" and add the database user and password.  
Set the apiAddress to the API address for your IOTA node
For MainNet, set minWeightMagnitude to 14.  For DevNet use 9
By default hub listens on 0.0.0.0:50051.  Modify your firewall accordingly.

Here is a sample shell script:

```
#!/bin/bash

./bazel-bin/hub/hub \
  --salt REPLACETHIS!!! \
  --db hub \
  --dbUser root \
  --dbPassword myrootpassword \
  --apiAddress 127.0.0.1:14265 \
  --minWeightMagnitude 14 \
  --listenAddress 127.0.0.1:50051
```

Change permissions to allow the script to execute

```chmod a+x start.sh```

Run hub

```./start.sh```

Now Hub is running 
