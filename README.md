**SOC Detection Lab**

This project demonstrates a Security Operations Center (SOC) detection lab built using Splunk.  
The lab focuses on log ingestion, attack simulation, and detection engineering.

 **Lab Overview**

**Technologies Used**
- Splunk Enterprise (SIEM)
- Splunk Universal Forwarder
- Windows Event Logs
- Linux SSH Authentication Logs
- Kali Linux (Attacker VM)

**Skills Demonstrated**
- Windows and Linux log ingestion
- Attack simulation
- Detection engineering
- Alert creation
- MITRE ATT&CK mapping


## Lab Architecture

- **Windows VM** → Endpoint log source  
- **Ubuntu VM** → Linux server log source  
- **Kali Linux** → Attack simulation  
- **Splunk** → Central SIEM  


## Attack Simulations

### 1️.SSH Brute Force (Linux)
- Simulated SSH brute-force attacks from Kali Linux
- Detected repeated failed login attempts in `/var/log/auth.log`
- Identified attacker IP and targeted usernames

### 2️.Encoded PowerShell Execution (Windows)
- Executed encoded PowerShell commands
- Detected suspicious PowerShell activity via Windows logs


##  Detection Engineering

### 1.SSH Brute Force Detection
- Created Sigma rule
- Converted detection logic to Splunk SPL
- Built a high-severity alert

**MITRE ATT&CK:** T1110 – Brute Force

### 2.SSH Invalid User Enumeration
- Detected repeated SSH attempts using invalid usernames
- Created medium-severity alert

**MITRE ATT&CK:** T1087 – Account Discovery



