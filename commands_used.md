# SOC Detection Lab — Commands Used

This document contains all commands used in this project, from Splunk setup to
attack simulation and detection engineering.

The purpose of this lab is to demonstrate real-world SOC analyst skills.

---

## PHASE 1 — Splunk Enterprise Setup (Ubuntu)

### Start Splunk Enterprise
sudo /opt/splunk/bin/splunk start --accept-license

### Enable Splunk Receiver (Port 9997)
sudo /opt/splunk/bin/splunk enable listen 9997

### Verify Receiver Is Listening
sudo ss -tulpn | grep 9997

---

## PHASE 2 — Windows Log Ingestion (Universal Forwarder)

### Check Forwarder Status
splunk.exe status

### Configure Forward Server
splunk.exe add forward-server 10.0.0.245:9997

### Verify Forwarding Connection
splunk.exe list forward-server

### Verify Windows Logs in Splunk
index=* sourcetype=WinEventLog*

---

## PHASE 3 — Ubuntu SSH Log Ingestion

### Download Splunk Universal Forwarder
wget -4 -O splunkforwarder.deb https://download.splunk.com/products/universalforwarder/releases/10.0.2/linux/splunkforwarder-10.0.2-linux-amd64.deb

### Install Forwarder
sudo dpkg -i splunkforwarder.deb

### Start Forwarder
sudo /opt/splunkforwarder/bin/splunk start --accept-license

### Add Splunk Server as Forward Target
sudo /opt/splunkforwarder/bin/splunk add forward-server 10.0.0.245:9997

### Monitor SSH Authentication Logs
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log

### Restart Forwarder
sudo /opt/splunkforwarder/bin/splunk restart

### Verify SSH Logs in Splunk
source="/var/log/auth.log"

---

## PHASE 4 — Attack Simulation

### SSH Brute Force (Kali → Ubuntu)
for i in {1..10}; do ssh fakeuser@10.0.0.245; done

### Verify Brute Force Logs
index=* "Failed password"

---

### Encoded PowerShell Execution (Windows)
powershell -EncodedCommand SQBtACAAaABhAGMAawBlAGQ=

### Verify PowerShell Logging
sourcetype="WinEventLog:Windows PowerShell" EncodedCommand

---

## PHASE 5 — Detection Engineering

### SSH Brute Force Detection Query
index=* "Failed password"
| stats count by src, user
| where count > 5

### Alert Configuration
Alert Name: SSH Brute Force Detected  
Trigger Condition: Number of results > 0  
Severity: High  

---

## Notes
- Logs collected from both Windows and Linux systems
- Real attack behavior simulated to generate telemetry
- Detections mapped to MITRE ATT&CK (T1110, T1059)

