
# Ubuntu System Preparation


- Check current hostname
hostnamectl

- Change hostname
sudo hostnamectl set-hostname wazuh_manager

- Verify Ubuntu version
lsb_release -a

# Update package lists and upgrade installed packages
sudo apt update && sudo apt upgrade -y

# ================================
# Download & Configure Wazuh Installer
# ================================

# Download installer and configuration files
wget <wazuh-install.sh_url>
wget <config.yml_url>

# Edit configuration file to set IPs for server, node, dashboard
nano config.yml

# Generate Wazuh configuration files using installer
sudo bash wazuh-install.sh -c config.yml

# ================================
# Wazuh Indexer Installation & Cluster Setup
# ================================

# Install Wazuh Indexer node
sudo bash wazuh-install.sh -i

# Start Wazuh Indexer service
sudo systemctl start wazuh-indexer

# Test Indexer cluster using curl
curl -u admin:<password> http://<server_ip>:9200

# ================================
# Troubleshooting Installation Issues
# ================================

# Install missing GPG keys
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <key>

# Resolve apt lock issues
sudo killall apt apt-get

# Reboot system if necessary
sudo reboot

# ================================
# Wazuh Manager Installation & Service Management
# ================================

# Install Wazuh Manager
sudo bash wazuh-install.sh -m

# Check status of Wazuh services
sudo /var/ossec/bin/wazuh-control status
systemctl status wazuh-manager

# Restart Wazuh services if needed
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard

# ================================
# Network & Port Verification
# ================================

# Verify Wazuh Manager listening ports
ss -lntp | grep 55000

# ================================
# Wazuh API Troubleshooting & Verification
# ================================

# Test API authentication
curl -k -u 'wazuh-wui:<password>' https://localhost:55000/security/user/authenticate

# Reinstall Wazuh Manager if API fails
sudo apt reinstall wazuh-manager

# ================================
# SQLite for API User Management
# ================================

# Install sqlite3
sudo apt install sqlite3

# Access Wazuh API user database
sudo sqlite3 /var/ossec/api/configuration/security/rbac.db

# View existing users
SELECT id, username FROM users;

# Update password hash for Wazuh UI user
UPDATE users SET password='PASTE_HASH_HERE' WHERE username='wazuh-ui';

# ================================
# Final Verification
# ================================

# Refresh dashboard in browser and verify API status
