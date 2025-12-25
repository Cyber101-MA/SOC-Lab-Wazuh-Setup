# Wazuh SOC Lab – Troubleshooting & Issue Resolution

This document provides a brief summary of the challenges encountered during the setup of the Wazuh SOC lab and how each issue was resolved. It can be used as a reference for troubleshooting similar environments.

---

## 1. GPG Key Missing & `apt` Lock Issue

**Problem:**  
During Wazuh Manager installation, the process failed due to missing GPG keys and an active `apt` process.

**Resolution:**  
- Installed the required GPG keys.  
- Restarted the system to clear the `apt` lock.  
- Re-ran the Wazuh installation script successfully.

---

## 2. Indexer Connector Initialization Failures

**Problem:**  
Wazuh Manager modules repeatedly failed to initialize Indexer Connectors for system processes, ports, hotfixes, hardware, and other mini managers.

**Resolution:**  
- Reinstalled the Wazuh Manager to ensure a clean configuration.  
- Restarted Wazuh services.  
- Verified Indexer status with `systemctl` to ensure full functionality.

---

## 3. Wazuh Dashboard Unable to Connect to Indexer

**Problem:**  
During initial dashboard installation, the dashboard could not connect to `node-1` (configured IP in `config.yml`) because the indexer was not fully operational.

**Resolution:**  
- Restarted the indexer using `systemctl`.  
- Verified the indexer was fully functional.  
- Restarted the dashboard and confirmed it was accessible and fully operational.

---

## 4. API Offline / Unauthorized Access

**Problem:**  
The Wazuh API was showing offline status and login attempts returned "Unauthorized".

**Resolution:**  
- Reinstalled the Wazuh Manager to reset API configuration.  
- Restarted all Wazuh services including the API.  
- Verified API status on the dashboard; it was online and functional after refreshing the page.  

---
