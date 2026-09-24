# KioskExpo7 Lab

> **Platform:** CyberDefenders  
> **Challenge:** KioskExpo7 Lab 
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/kioskexpo7/
> **Writeup Completed:** 

## Executive Summary
On October 18, 2025, we understand from staff that laptops were being used as kiosks to display QR codes, allowing attendees to sign in. This was subject to abuse, where an attacker likely bypassed the kiosk lockdown by using keyboard shortcuts to expose a Help button, which led them to a fully functioning address bar. From browser history, we were able to determine that they reached qr-code.io to create a malicious QR code that would redirect event attendees to a malicious site. Since only Edge could be run in kiosk mode, they used the Edge address bar to access the file system using file:///C:/. They downloaded a copy of cmd.exe, which they renamed to msedge.exe. The legitimate qr-code.png was moved away, and qr.png was renamed to qr-code.png and placed on the Desktop. The kiosk was then displaying a malicious QR code pointing to https://registerr[.]wowzaconf[.]dev/register[.]php. The attacker also maintained persistence by creating alive.ps1 and update.ps1 in C:\ProgramData\Maintenance and registering them as scheduled tasks named KioskStatusCheck and KioskUpdate. The alive.ps1 script beaconed system information to the C2 server, while update.ps1 could retrieve and execute an additional payload called quickupdate.txt.

## Initial Access
Kiosk Escape - This involves abusing browser shortcuts (such as Ctrl+O, Ctrl+S, or Ctrl+P) to invoke File Explorer, then clicking the Help button to spawn an unrestricted browser instance.

## Attack Timeline
| Stage | Event |
|---|---|
| Initial Access | The attacker physically approaches the kiosk and abuses browser shortcuts to invoke File Explorer. They then click the Help button, which spawns an unrestricted browser instance with a fully functioning address bar. |
| Execution | The attacker downloads `cmd.exe` and renames the executable to `msedge.exe`, allowing them to bypass the kiosk restriction and launch `msedge.exe` (`cmd.exe`). From there, they launch PowerShell under the `kiosk` user. |
| Discovery / Privilege Escalation | Inside the PowerShell session running under the `kiosk` user, the attacker runs an enumeration script which obtains the `KioskAdmin` credentials. |
| Privilege Escalation | The attacker runs `runas /user:KioskAdmin powershell` and manually approves the UAC elevation prompt, giving them elevated administrative access. |
| Defense Evasion | The attacker disables UAC by setting `EnableLUA` to `0` in the registry to prevent future UAC prompts. |
| Impact | The attacker replaces the legitimate QR code. The legitimate `qr-code.png` is moved away, while the malicious `qr.png` is renamed to `qr-code.png` and placed on the Desktop. |
| Persistence / Command & Control | The attacker creates `alive.ps1` and `update.ps1` in `C:\ProgramData\Maintenance` and registers them as scheduled tasks. `alive.ps1` functions as a beacon, while `update.ps1` provides Command & Control functionality. |
| Anti-Forensics | The attacker attempts to overwrite the `KioskAdmin` PowerShell history and deletes the disguised `cmd.exe` / `msedge.exe`, which is moved to the Recycle Bin as `$R0BD893.exe`. |

## Key Evidence
| Artifact | Information Found |
|---|---|
| Edge History | Showed the kiosk breakout activity, browsing to local `file:///` paths, and the download of `cmd.exe`. |
| USN Journal / `$MFT` | Showed `cmd.exe` being renamed to `msedge.exe`, the malicious QR file being renamed and placed on the Desktop, and the deleted executable appearing in the Recycle Bin. |
| PowerShell History | Showed the download and execution of the enumeration script and the command `runas /user:KioskAdmin powershell`. |
| Registry | Showed the kiosk `RestrictRun` configuration, the stored `KioskAdmin` credentials, and `EnableLUA` being set to `0`. |
| Prefetch | Confirmed execution of `msedge.exe`, PowerShell, and other utilities used during the attack. |
| Security Event Log | Confirmed the use of explicit credentials for the `KioskAdmin` account. |
| PowerShell Operational Log | Showed the commands used to create the `KioskStatusCheck` and `KioskUpdate` scheduled tasks. |
| MFT Resident Data | Allowed recovery of `alive.ps1` and `update.ps1`, revealing their beaconing and C2 functionality. |


## MITRE ATT&CK Mapping
| Technique | ID | Description |
|---|---|---|
| Masquerading: Rename Legitimate Utilities | T1036.003 | The attacker renamed `cmd.exe` to `msedge.exe` to bypass the kiosk's filename-based application restriction. |
| Command and Scripting Interpreter: Windows Command Shell | T1059.003 | The attacker executed the renamed `cmd.exe` to gain command-line access to the kiosk. |
| Command and Scripting Interpreter: PowerShell | T1059.001 | PowerShell was used for enumeration, privilege escalation activity, persistence scripts and C2 functionality. |
| Ingress Tool Transfer | T1105 | The attacker downloaded tools and payloads onto the system, including `lightpeas.bat` and additional files used during the compromise. |
| Unsecured Credentials: Credentials in Registry | T1552.002 | The attacker discovered the `KioskAdmin` credentials stored insecurely in the Windows Registry. |
| Valid Accounts: Local Accounts | T1078.003 | The attacker used the legitimate local `KioskAdmin` account to obtain higher privileges. |
| Modify Registry | T1112 | The attacker modified the `EnableLUA` registry value to disable UAC. |
| Scheduled Task/Job: Scheduled Task | T1053.005 | The attacker created the `KioskStatusCheck` and `KioskUpdate` scheduled tasks to execute malicious PowerShell scripts and maintain persistence. |
| Application Layer Protocol: Web Protocols | T1071.001 | The persistence scripts communicated with attacker-controlled infrastructure using web protocols for beaconing and C2 activity. |
| Indicator Removal: Clear Command History | T1070.003 | The attacker overwrote the `KioskAdmin` PowerShell command history to remove evidence of executed commands. |
| Indicator Removal: File Deletion | T1070.004 | The attacker deleted tools used during the compromise, including the enumeration script and disguised `cmd.exe`. |

## Detection Opportunities
| Detection Opportunity | Purpose |
|---|---|
| Detect downloads of executable files or scripts | A kiosk should not normally be downloading tools or executables. Downloads such as `.exe`, `.bat`, or `.ps1` files should be treated as unusual and investigated. |
| Detect creation or modification of scheduled tasks | Scheduled tasks should rarely change on a kiosk. New or modified tasks could indicate an attempt to establish persistence. |
| Detect sensitive registry modifications | Monitor for changes to security-related registry values such as `EnableLUA`, application restriction settings, or other configuration areas that should remain static on a kiosk. |
| Detect unusual file creation, rename, or deletion activity | Changes to files used by the kiosk, such as the QR code or files within protected application directories, could indicate tampering. |
| Detect connections to known malicious or unusual external infrastructure | Kiosk systems should have a limited set of expected network destinations. Connections to known malicious IP addresses, rare domains, or unexpected external services could indicate C2 activity. |

## Recommendations and Lessons Learned
- Harden kiosk mode so that File Explorer dialogs cannot be abused to escape the restricted environment.
- Use application control based on file hashes, signatures, or trusted publishers rather than filename alone.
- Remove plaintext administrator credentials from the registry.
- Restrict outbound network connections to approved destinations only.
- Prevent the kiosk account from accessing PowerShell, Command Prompt, and other command interpreters.
- Protect kiosk content such as `qr-code.png` from unauthorised modification using appropriate file permissions, integrity controls, or read-only deployment mechanisms.
- Treat public-facing kiosk systems as high-risk assets and monitor them for activity that falls outside their expected behaviour.

## Tools Used
