# Rogue Azure Lab
> **Platform:** CyberDefenders
> **Challenge:** Rogue Azure Lab  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/rogue-azure/

## Executive Summary
On November 14, 2025, security monitoring detected suspicious authentication activity within the Azure tenant, with sign-in attempts coming from multiple geographic locations. Using Microsoft Sentinel, we identified the source of a password spray attack as `52.59.240.166`, which successfully gained access to the `mharmon@compliantsecure.store` account.

Investigation finds that a compromised administrator account was used to elevate `lwilliams@compliantsecure.store` to Global Administrator role.  We were unable to determine how the attacker gained access to the `lwilliams` account.

The attacker maintained persistence by creating two applications, `OfficeRead` and `VaultApp`, with `VaultApp` being used to access directory information. The attacker then started using the Singapore-based IP address `52.221.180.165`, accessed the `mainstoragestore01` storage account and downloaded `Confidential.png`.

## Initial Access
Password Spray from 52.59.240.166



## Timeline
1. Password Spray from 52.59.240.166

2. Compromised mharmon@compliantsecure.store account

3. The attacker maintained persistence by creating 'OfficeRead' & 'VaultApp'

4. Privilege escalation, the attacker escalated lwilliams@compliantsecure.store to a Global Administrator role

5. Command and Control contact, the attacker connected to a C2 IP `52.221.180.165` based in Singapore

6. The attacker accessed the storage account mainstoragestore01 and exfiltrated Confidintal.png


## Key Evidence
| Evidence | Significance |
|----------|--------------|
| 52.59.240.166 | Initial Access Password spray IP |
| mharmon@compliantsecure.store | Initial Access first account compromised |
| 52.221.180.165 | C2C IP in Singapore |
| lwilliams@compliantsecure.store | Account escalated to Global Administrator |
| OfficeRead | Persistence App |
| VaultApp | Persistence App |
| mainstoragestore01 | storage account accessed |
| Confidential.png | Data exfiltrated |

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|----------|------------|------------------------------------------------------------|
| Brute Force / Password Spray | T1110.003 | Password Spray from 52.59.240.166 which resulted in initial access |
| Access to valid domain account | T1078.002 | Password spray resulted in access to 'mharmon@compliantsecure.store' |
| Privilege escalation / Valid Accounts: Domain accounts| T1078.002 | 'lwilliams@compliantsecure.store' Account escalated to Global Administrator | 
| Cloud application integration | T1671 | OfficeRead & VaultApp created |
| Data from Cloud Storage | T1530 | mainstoragestore01 account accessed |
| Exfiltration Over C2 Channel | T1041 | Confidential.png exfiltrated over 52.221.180.165 | 


## Detection Opportunities
| Detection | Purpose |
|-----------|---------|
| Monitor for failed logins from a single IP in a 15 minute period | Detects password spray |
| Monitor Network connections to C2 IPs | Detects C2 communication |
| Monitor job for App creation | Detects unusual users creating accounts |
| Monitor Privilege escalation | Detects unauthorised privilege escalation |
| Monitor users accessing storage accounts | Detects if the correct user is accessing the storage |

## Recommendations
- Block logins from C2 IPs
- Enable MFA

## Lessons Learned
- This investigation demonstrates the importance of enabling MFA and being proactive on threat intelligence to block known bad IPs. 

## Tools Used
- Microsoft Sentinel
- KQL

