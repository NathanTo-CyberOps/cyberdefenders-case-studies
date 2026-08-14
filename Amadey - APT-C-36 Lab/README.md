# Amadey - APT-C-36 Lab
> **Platform:** CyberDefenders
> **Challenge:** Amadey - APT-C-36 Lab  
> **Achievement:** https://cyberdefenders.org/blueteam-ctf-challenges/achievements/BigPuffer/amadey-apt-c-36/
> **Write up completed:** 18/07/2026

## Executive Summary
An EDR alert was triggered due to the detection of the Amadey Trojan Stealer. The exact initial access could not be determined from the available memory image. However, lssass.exe was executed from the user's Temp directory and established persistence by creating a copy in the Scheduled Tasks directory. The malware then communicated with the C2 server 41.75.84.12 over HTTP (TCP/80), where it downloaded the payloads cred64.dll and clip64.dll. Finally, lssass.exe spawned rundll32.exe to execute clip64.dll from the user's AppData\Roaming directory.

## Initial Access
The initial access vector could not be determined from the available memory image.

## Timeline
1. Initial access could not be determined from the available memory image.

2. The malicious executable lssass.exe was executed from the user's temporary directory.

3. The malware established persistence by creating an additional copy within the Windows Scheduled Tasks directory.

4. lssass.exe established communication with the command-and-control (C2) server at 41.75.84.12 over HTTP (TCP/80).

5. The malware downloaded the additional payloads cred64.dll and clip64.dll from the C2 server.

6. lssass.exe spawned rundll32.exe to execute clip64.dll from the user's AppData\Roaming directory.

## Key Evidence
| Evidence | Significance |
|----------|--------------|
| lssass.exe | Primary malicious executable |
| 41.75.84.12 | Command-and-control server |
| TCP/80 | HTTP communication used for payload delivery |
| cred64.dll | Downloaded payload |
| clip64.dll | Downloaded payload executed through rundll32.exe |
| rundll32.exe | LOLBin used to execute malicious DLL |

## MITRE ATT&CK Mapping
| Technique | ID | Description |
|----------|------------|------------------------------------------------------------|
| Ingress Tool Transfer | T1105 | The malware downloads additional payloads (`cred64.dll` and `clip64.dll`) from the C2 server. |
| Application Layer Protocol: Web Protocols | T1071.001 | The malware communicates with the C2 server over HTTP (TCP/80). |
| Registry Run Keys / Startup Folder | T1547.001 | Amadey is capable of establishing persistence through Registry Run keys and Startup folders. |
| System Information Discovery | T1082 | The malware gathers information about the victim system. |
| File and Directory Discovery | T1083 | The malware enumerates files and directories on the compromised host. |
| Security Software Discovery | T1518.001 | The malware checks for installed security software and antivirus products. |
| Obfuscated Files or Information | T1027 | The malware obfuscates strings and other data to hinder analysis. |
| Deobfuscate/Decode Files or Information | T1140 | The malware decodes or decrypts embedded strings during execution. |
| System Language Discovery | T1614 | The malware checks the victim's system location and terminates if the host is located in Russia. |

## Detection Opportunities
The Amadey Malware could be detected on suspicious execution paths e.g. Temp directory. This can also be detected from persistence mechanisms like modifying the scheduled tasks.

This can also be detected through network indicators such as C2 Traffic and payload downloads

## Recommendations
- Block known malicious IP addresses and domains.
- Restrict execution from user-writable directories such as Temp and AppData where possible.
- Monitor and alert on suspicious Scheduled Task creation.
- Deploy endpoint detection capable of identifying LOLBin abuse, including `rundll32.exe`.
- Maintain up-to-date threat intelligence to detect known Amadey infrastructure.

## Lessons Learned
This investigation demonstrates the importance of monitoring execution from user-writable directories and identifying the abuse of legitimate Windows utilities such as `rundll32.exe`. Combining endpoint telemetry with network indicators enables investigators to reconstruct malware execution, persistence and command-and-control activity, even when the initial access vector cannot be determined.

## Tools Used
- Volatility 3
- MemProcFS
- PEStudio
- VirusTotal
