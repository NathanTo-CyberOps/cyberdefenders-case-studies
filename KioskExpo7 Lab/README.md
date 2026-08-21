# KioskExpo7 Lab

> **Platform:** CyberDefenders  
> **Challenge:** KioskExpo7 Lab 
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/kioskexpo7/
> **Writeup Completed:** 

## Executive Summary
On October 18, 2025, we understand from staff that laptops were being used as kiosks to display QR codes, allowing attendees to sign in. This was subject to abuse, where an attacker likely bypassed the kiosk lockdown by using keyboard shortcuts to expose a Help button, which led them to a fully functioning address bar. From browser history, we were able to determine that they reached qr-code.io to create a malicious QR code that would redirect event attendees to a malicious site. Since only Edge could be run in kiosk mode, they used the Edge address bar to access the file system using file:///C:/. They downloaded a copy of cmd.exe, which they renamed to msedge.exe. The legitimate qr-code.png was moved away, and qr.png was renamed to qr-code.png and placed on the Desktop. The kiosk was then displaying a malicious QR code pointing to https://registerr[.]wowzaconf[.]dev/register[.]php. The attacker also maintained persistence by creating alive.ps1 and update.ps1 in C:\ProgramData\Maintenance and registering them as scheduled tasks named KioskStatusCheck and KioskUpdate. The alive.ps1 script beaconed system information to the C2 server, while update.ps1 could retrieve and execute an additional payload called quickupdate.txt.

## Initial Access
Kiosk Escape - This involves abusing browser shortcuts (such as Ctrl+O, Ctrl+S, or Ctrl+P) to invoke File Explorer, then clicking the Help button to spawn an unrestricted browser instance.

## Attack Chain
| Stage | Activity |
|-------|----------|
|       |          |

## Timeline
| Step | Event |
|------|-------|
|      |       |

## Key Evidence
| Evidence | Significance |
|----------|--------------|
|          |              |


## MITRE ATT&CK Mapping
| Technique | ID | Description |
|-----------|----|-------------|
|           |    |             |

## Detection Opportunities
| Detection | Purpose |
|-----------|---------|
|           |         |

## Recommendations

## Lessons Learned

## Tools Used
