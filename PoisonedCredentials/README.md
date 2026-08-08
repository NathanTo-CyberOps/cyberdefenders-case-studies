# PoisonedCredentials

> **Platform:** CyberDefenders
> **Challenge:** PoisonedCredentials  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/poisonedcredentials/

## Executive Summary
This alert was triggered due to suspicious network activity involving the abuse of the LLMNR and NBT-NS protocols. When a user mistypes a hostname, an LLMNR broadcast is sent across the network to resolve the unknown name. An attacker can respond before the legitimate host, causing the victim to connect to a rogue SMB server. Once the victim attempts NTLM authentication, the attacker can capture the NTLM challenge-response for potential credential compromise.

## Initial Access
| Stage | Activity |
|------|----------|
| 1 | Victim mistypes hostname |
| 2 | LLMNR broadcast |
| 3 | Attacker responds |
| 4 | Victim connects to rogue SMB server |
| 5 | NTLM authentication attempted |
| 6 | Credentials captured |

## Timeline
| Step | Event |
|------|-------|
| 1 | A user on **192.168.232.162** mistyped the hostname **"fileshaare"**, triggering an LLMNR broadcast. |
| 2 | Analysis of the LLMNR traffic identified **192.168.232.215** as the rogue host responding to the broadcast. |
| 3 | The filter `nbns.addr == 192.168.232.215` identified **192.168.232.176** as the second affected host communicating with the attacker. |
| 4 | SMB2 authentication traffic destined for **192.168.232.176** revealed the compromised user account **janesmith**. |
| 5 | SMB2 traffic destined for **192.168.232.215** identified the compromised workstation hostname as **AccountingPC**. |

## Key Evidence
| Evidence | Significance |
|----------|--------------|
| LLMNR query for the hostname **"fileshaare"** | Indicates the initial failed hostname lookup that triggered the attack. |
| LLMNR response from **192.168.232.215** | Identifies the rogue system responding to the broadcast request. |
| NBNS traffic identifying **192.168.232.176** | Identifies the affected host communicating with the attacker. |
| SMB2 authentication packets | Reveal the compromised user account **janesmith**. |
| SMB2 traffic | Identifies the compromised workstation as **AccountingPC**. |

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|-----------|------------|--------------------------------------------------------------|
| LLMNR/NBT-NS Poisoning and SMB Relay | T1557.001 | The attacker responded to LLMNR/NBT-NS name resolution requests to impersonate a legitimate host and capture NTLM authentication attempts. |
| Forced Authentication | T1187 | The victim was induced to authenticate to the attacker's rogue SMB server after the poisoned LLMNR response. |

## Detection Opportunities
| Detection | Purpose |
|-----------|---------|
| Monitor LLMNR and NBT-NS traffic within the network. | Detect unusual name resolution activity. |
| Alert on LLMNR responses from non-authorised hosts. | Identify potential rogue responders performing name resolution poisoning. |
| Detect repeated failed hostname resolution requests. | Identify systems vulnerable to LLMNR/NBT-NS poisoning attacks. |
| Alert on SMB authentication to unexpected hosts. | Detect victims attempting to authenticate to rogue SMB servers. |
| Disable LLMNR and NBT-NS where operationally possible. | Prevent attackers from abusing legacy name resolution protocols. |

## Recommendations
- Disable LLMNR where operationally possible.
- Disable NetBIOS over TCP/IP where legacy support is not required.
- Enable SMB signing.
- Implement network segmentation.
- Monitor for rogue name resolution responses.
- Use strong password policies and multi-factor authentication to reduce the impact of credential theft.

## Lessons Learned
This investigation demonstrated how legacy name resolution protocols can be abused to capture user credentials without exploiting software vulnerabilities. It also highlighted the importance of analysing network traffic chronologically to reconstruct attacker activity and identify compromised hosts.

## Tools Used
- Wireshark
- Volatility 3
- MemProcFS
- PEStudio
- VirusTotal
- CyberChef
- Chainsaw
- Sigma CLI
