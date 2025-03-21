---
title: "Powershell"
date: 2025-03-21T21:22:12+01:00
draft: false
hidden: false
weight: 4
tags:
    - microsoft
    - powershell
    - cheatsheet
summary: ""
---

|Revised Date | Author | Comment |
| ----------- | ------ | ------- |
| 21.03.2025  | Roger Johnsen | Article added |

## Introduction

**Understanding PowerShell flags and commonly abused cmdlets is vital for threat hunters as attackers frequently exploit them for stealthy execution, data exfiltration, and persistence. This knowledge helps identify malicious activity, map it to known attack techniques like those in MITRE ATT&CK, and strengthen detection and response strategies against threats effectively.**

---

## Powershell Command Flags and Cmdlets

### **PowerShell Command Flags**

| **Flag**               | **Description**                                                                                          | **MITRE ATT&CK Technique**                                                                                     |
|-------------------------|----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `-ExecutionPolicy`      | Overrides the execution policy for the session. Example: `-ExecutionPolicy Bypass`.                     | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `-EncodedCommand`       | Executes a base64-encoded command. Example: `-EncodedCommand <BASE64_STRING>`.                          | [T1027 (Obfuscated Files or Information)](https://attack.mitre.org/techniques/T1027/), [T1059.001](https://attack.mitre.org/techniques/T1059/001/) |
| `-NoProfile`            | Runs PowerShell without loading user-specific profiles.                                                 | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `-File`                 | Runs a specified script file. Example: `-File script.ps1`.                                              | [T1204.002 (Malicious File)](https://attack.mitre.org/techniques/T1204/002/)                                   |
| `-Command`              | Executes specified commands directly. Example: `-Command Write-Host "Hello, world!"`.                   | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `-WindowStyle`          | Controls the window appearance. Example: `-WindowStyle Hidden`.                                         | [T1564.003 (Hide Artifacts)](https://attack.mitre.org/techniques/T1564/003/)                                   |
| `-InputFormat`          | Specifies the format of input data, either `Text` or `XML`.                                             | [T1123 (Audio Capture)](https://attack.mitre.org/techniques/T1123/), depending on malicious use.               |
| `-OutputFormat`         | Specifies the format of output data, either `Text` or `XML`.                                            | [T1020 (Data Transfer)](https://attack.mitre.org/techniques/T1020/)                                            |
| `-Version`              | Specifies the version of PowerShell to run. Example: `-Version 2.0`.                                    | [T1070.004 (Indicator Removal on Host)](https://attack.mitre.org/techniques/T1070/004/)                        |
| `-NoExit`               | Keeps the PowerShell session open after execution.                                                      | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |


### **Commonly Abused Cmdlets**

| **Cmdlet**              | **Description**                                                                                          | **MITRE ATT&CK Technique**                                                                                     |
|-------------------------|----------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| `Invoke-Expression`     | Executes a string as a PowerShell command.                                                              | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `Invoke-WebRequest`     | Downloads files or sends requests over the web.                                                         | [T1105 (Ingress Tool Transfer)](https://attack.mitre.org/techniques/T1105/)                                    |
| `Start-Process`         | Starts a new process on the system.                                                                     | [T1543 (Create or Modify System Processes)](https://attack.mitre.org/techniques/T1543/)                        |
| `New-Object`            | Creates and initializes a .NET object, often used maliciously to download files or execute code.        | [T1129 (Execution through Module Load)](https://attack.mitre.org/techniques/T1129/)                            |
| `Import-Module`         | Loads a PowerShell module.                                                                              | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `Set-ExecutionPolicy`   | Changes the PowerShell execution policy, potentially disabling security controls.                        | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `Add-Member`            | Adds properties or methods to an object. Used for advanced customization, sometimes maliciously.        | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
| `Get-Credential`        | Prompts for user credentials in a secure dialog box.                                                    | [T1056.004 (Credential API Hooking)](https://attack.mitre.org/techniques/T1056/004/)                           |
| `Export-Csv`            | Exports data to a CSV file.                                                                             | [T1020 (Data Transfer)](https://attack.mitre.org/techniques/T1020/)                                            |
| `Where-Object`          | Filters objects in the pipeline. May be used in scripts to obfuscate data collection.                   | [T1059.001 (PowerShell)](https://attack.mitre.org/techniques/T1059/001/)                                       |
