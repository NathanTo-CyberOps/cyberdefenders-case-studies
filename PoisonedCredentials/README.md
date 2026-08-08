# PoisonedCredentials

> **Platform:** CyberDefenders
> **Challenge:** PoisonedCredentials  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/poisonedcredentials/

## Executive Summary
This investigation examines an incident involving poisoned credentials, where an attacker exploited vulnerabilities in the LLMNR and NBT-NS protocols to impersonate a legitimate host and capture authentication attempts. Using Wireshark, malicious LLMNR traffic was analysed to identify the rogue system responsible for responding to name resolution requests. Further analysis identified the compromised host and user account that communicated with the attacker. The investigation reconstructs the attack and highlights how insecure name resolution protocols can be abused to obtain user credentials.

## Incident Overview
The investigation is based on a packet capture (PCAP) containing suspicious network activity within an enterprise environment. The objective is to identify the attacker, determine how the credential poisoning attack was performed, identify the affected systems and accounts, and reconstruct the attack using network evidence.

## Initial Access
The attacker exploited the LLMNR and NBT-NS name resolution protocols by responding to a failed hostname lookup before the legitimate host could reply. This poisoned the victim's name resolution request, causing authentication attempts to be redirected to the attacker's SMB service.

Victim mistypes hostname
        ↓
LLMNR broadcast
        ↓
Attacker responds
        ↓
Victim connects to rogue SMB server
        ↓
NTLM authentication attempted
        ↓
Credentials captured

## Timeline
This investigation examines a surge in suspicious network activity within an organisation where the attacker's actions were first noticed after a user mistyped the query "fileshaare" from the machine with the IP address "192.168.232.162". Analysis of the LLMNR traffic showed that the rogue system responding to the request had the IP address 192.168.232.215, identifying it as the attacker's machine. We then attempted to identify a second affected host by applying the filter "nbns.addr == 192.168.232.215", which revealed the IP address "192.168.232.176". To identify the compromised account, SMB2 packets destined for "192.168.232.176" were analysed, revealing the username "janesmith". Finally, to determine which host the attacker accessed over SMB, the filter "ip.dst == 192.168.232.215" and smb2 was applied, identifying the hostname AccountingPC.

## Key Evidence
- LLMNR query for the non-existent hostname "fileshaare".
- LLMNR response originating from 192.168.232.215.
- NBNS traffic identifying the affected host at 192.168.232.176.
- SMB2 authentication packets revealing the username janesmith.
- SMB2 traffic identifying the hostname AccountingPC.

## Detection Opportunities
- Monitor LLMNR and NBT-NS traffic within enterprise networks.
- Alert on LLMNR responses from non-authorised hosts.
- Detect repeated name resolution failures.
- Disable LLMNR and NBT-NS where possible.
- Alert on SMB authentication to unexpected hosts.

## Recommendations
- Disable LLMNR where operationally possible.
- Disable NetBIOS over TCP/IP where legacy support is not required.
- Enable SMB signing.
- Implement network segmentation.
- Monitor for rogue name resolution responses.
- Use strong password policies and multi-factor authentication to reduce the impact of credential theft.

## Lessons Learned
This investigation demonstrated how legacy name resolution protocols can be abused to capture user credentials without exploiting software vulnerabilities. It also highlighted the importance of analysing network traffic chronologically to reconstruct attacker activity and identify compromised hosts.
