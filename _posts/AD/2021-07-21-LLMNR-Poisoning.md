---
title: "LLMNR Poisoning"
excerpt: An article to show LLNMR Poisoning
categories:
  - ActiveDirectory
read_time: true
words_per_minute: 70
header:
      overlay_image: /assets/imgs/hacker3.jpg
---


# LLMNR Poisoning

LLMNR - The link-local multicast name resolution. A protocol with a similar function as that of NetBIOS (Outdated) and generally, is used as a replacement.  
LLMNR and NetBIOS allow name resolution for devices on the network, essentially - A DNS type function, without needing DNS configuration. As LLMNR sends its requests via broadcast packets, it is possible to intercept them and perform a Man-in-the-Middle (MITM) attack.


## Example Attack:

First things first a lab is required for the example attack. For the example attack below the following config was used:

**Machines**
- A Domain Controller running active directory.
- A Windows 10 (Education) workstation added to the domain network.
- Parrot OS machine, not added to the domain, however, still on the same network.

**Tools**
- Responder
- Hashcat

**How the attack works**
- Victim user will try to access something like a drive, which is spelt in correctly or not resolvable via DNS.
- Responder will act as a DNS server "knowing" where this drive is.
- Victim sends over NTLMv2 username and password hash.
- Password is cracked with Hashcat. 

Firstly, start Responder. Keep it running until traffic is received.

For this attack the options -rdw and -v are used. 
- r = Enables Responder to answer NetBIOS wredir queries.
- d = Enables Responder to answer NetBIOS domain suffix queries.
- w = Start the WPAD server.
- v = Verbose, so that captures can be handled as they come in.

![Responder](/assets/imgs/Posts/LLMNR/Responder.png)

The user victim mistakenly tries to connect to the network drive "marketig". This causes LLMNR to broadcast packets to the entire network to see if there is any machine that knows the location of this drive.
![Responder Catch](/assets/imgs/Posts/LLMNR/Responder_Catch.png)

Looking at the NTLMv2 hash, it is possible to see its format - Username::Domain:Password-Hash. Knowing that the hash is NTLMv2, cracking the hash is an option. By cracking the hash, it is possible to open a door into the network. Getting a user and their password allows for a potential pivot in the network.

Using Hashcat, it is possible to crack the password using the module 5600.
![Responder Catch](/assets/imgs/Posts/LLMNR/Hashcat.png)

And behold... the password of Password123 has been uncovered.