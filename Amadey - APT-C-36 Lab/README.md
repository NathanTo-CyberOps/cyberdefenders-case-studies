# Amadey - APT-C-36 Lab

## Executive Summary
EDR triggered due to the detection of an Amadey Trojan stealer, the exact initial access is unable to be determined from the evidence collected from the memory dump. However, after the malware had been downloaded lssass.exe was executed in the users Temp directory and in the Scheduled tasks directory. lssass.exe also established connection to the C2 server 41.75.84.12 on port 80 where cred64.dll and clip64.dll were downloaded. Then lssass.exe spawned rundll32.exe to execute clip64.dll from the AppData\Roaming directory.

## Initial Access
Unknown - Unable to determine from the evidence found

## Attack Chain

## Timeline
1. Initial access could not be determined from the available memory image.

2. The malicious executable lssass.exe was executed from the user's temporary directory.

3. The malware established persistence by creating an additional copy within the Windows Scheduled Tasks directory.

4. lssass.exe established communication with the command-and-control (C2) server at 41.75.84.12 over HTTP (TCP/80).

5. The malware downloaded the additional payloads cred64.dll and clip64.dll from the C2 server.

6. lssass.exe spawned rundll32.exe to execute clip64.dll from the user's AppData\Roaming directory.

## Key Evidence

## MITRE ATT&CK Mapping

## Detection Opportunities

## Recommendations

## Lessons Learned
