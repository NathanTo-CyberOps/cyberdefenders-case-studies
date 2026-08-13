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

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|----------|------------|------------------------------------------------------------|


## Detection Opportunities


## Recommendations


## Lessons Learned


## Tools Used

