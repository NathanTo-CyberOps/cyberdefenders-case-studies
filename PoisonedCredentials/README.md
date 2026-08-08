# PoisonedCredentials

> **Platform:** CyberDefenders
> **Challenge:** PoisonedCredentials  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/poisonedcredentials/

## Executive Summary
This investigation examines an incident involving poisoned credentials, where an attacker exploited vulnerabilities in the LLMNR and NBT-NS protocols to impersonate a legitimate host and capture authentication attempts. Using Wireshark, malicious LLMNR traffic was analysed to identify the rogue system responsible for responding to name resolution requests. Further analysis identified the compromised host and user account that communicated with the attacker. The investigation reconstructs the attack and highlights how insecure name resolution protocols can be abused to obtain user credentials.

## Incident Overview

## Initial Access
Unauthenticated SMB

## Attack Chain
Unauthenticated SMB access -> stealing password credentails

## Timeline
This investigation examines a surge in suspicious network activity within an organisation where the attacker's actions were first noticed after a user mistyped the query "fileshaare" from the machine with the IP address "192.168.232.162". Analysis of the LLMNR traffic showed that the rogue system responding to the request had the IP address 192.168.232.215, identifying it as the attacker's machine. We then attempted to identify a second affected host by applying the filter "nbns.addr == 192.168.232.215", which revealed the IP address "192.168.232.176". To identify the compromised account, SMB2 packets destined for "192.168.232.176" were analysed, revealing the username "janesmith". Finally, to determine which host the attacker accessed over SMB, the filter "ip.dst == 192.168.232.215" and smb2 was applied, identifying the hostname AccountingPC.

## Key Evidence

## MITRE ATT&CK Mapping
T1078 Valid accounts
T1036 Masquerading
T1589 Gather Victim Identity Information
T1212 Exploitation for Credential Access

## Detection Opportunities
Querying hosts that don't exist

## Recommendations
Stronger authentication controls

## Lessons Learned
