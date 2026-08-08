# Amadey - APT-C-36 Lab

## Executive Summary

## Incident Overview

## Initial Access

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
