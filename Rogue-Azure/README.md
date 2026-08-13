# Rogue Azure Lab
> **Platform:** CyberDefenders
> **Challenge:** Rogue Azure Lab  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/rogue-azure/

## Executive Summary
On November 14, 2025, security monitoring detected suspicious authentication activity within the Azure tenant, with sign-in attempts coming from multiple geographic locations. Using Microsoft Sentinel, we identified the source of a password spray attack as `52.59.240.166`, which successfully gained access to the `mharmon@compliantsecure.store` account.

Further investigation identified the compromised administrator account `lwilliams@compliantsecure.store`, which was used to elevate the privileges of another user by assigning them the Global Administrator role. From the available evidence, we were unable to determine how the attacker gained access to the `lwilliams` account.

The attacker maintained persistence by creating two applications, `OfficeRead` and `VaultApp`, with `VaultApp` being used to access directory information. The attacker then started using the Singapore-based IP address `52.221.180.165`, accessed the `mainstoragestore01` storage account and downloaded `Confidential.png`.



## Initial Access
| Stage | Activity |
|------|----------|



## Timeline


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

