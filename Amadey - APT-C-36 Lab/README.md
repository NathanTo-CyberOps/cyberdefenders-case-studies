# Amadey - APT-C-36 Lab

## Executive Summary
EDR triggered due to the detection of an Amadey Trojan stealer, the exact initial access is unable to be determined from the evidence collected from the memory dump. However, after the malware had been downloaded lssass.exe was executed in the users Temp directory and in the Scheduled tasks directory. lssass.exe also established connection to the C2 server 41.75.84.12 on port 80 where cred64.dll and clip64.dll were downloaded. Then lssass.exe spawned rundll32.exe to execute clip64.dll from the AppData\Roaming directory.

## Initial Access
Unknown - Unable to determine from the evidence found

## Attack Chain
Initial Access Unknown -> Malware execution -> Payload download -> Payload execution

## Timeline
1. Initial access could not be determined from the available memory image.

2. The malicious executable lssass.exe was executed from the user's temporary directory.

3. The malware established persistence by creating an additional copy within the Windows Scheduled Tasks directory.

4. lssass.exe established communication with the command-and-control (C2) server at 41.75.84.12 over HTTP (TCP/80).

5. The malware downloaded the additional payloads cred64.dll and clip64.dll from the C2 server.

6. lssass.exe spawned rundll32.exe to execute clip64.dll from the user's AppData\Roaming directory.

## Key Evidence
lssass.exe
41.75.84.12/:80
cred64.dll 
clip64.dll

## MITRE ATT&CK Mapping
Command and Control (T1071.001): Uses application layer protocols like HTTP for C2 communications.
Persistence & Defense Evasion (T1547.001, T1112): Modifies and overwrites registry run keys and startup folder items.
Discovery (T1082, T1083, T1518.001): Performs system information gathering, file/directory discovery, and security software/antivirus discovery.
Execution & Ingress Tool Transfer (T1106, T1105): Leverages native Windows APIs and downloads/executes additional files or malicious payloads.
Obfuscation (T1027, T1140): Obfuscates and decodes strings such as domains and security vendor names.Localization Control (T1614): Checks victim system settings and halts execution if the machine is located in Russia.

## Detection Opportunities
The Amadey Malware could be detected on suspicious execution paths e.g. Temp directory. This can also be detected from persistence mechanisms like modifying the scheduled tasks.

This can also be detected through network indicators such as C2 Traffic and payload downloads

## Recommendations

## Lessons Learned
Detections on execution in suspicious file path, Bad IP can prevent the malicious payload from being downloaded
