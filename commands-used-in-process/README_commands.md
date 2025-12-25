# --- System Preparation & Prerequisites ---
hostnamectl
sudo hostnamectl set-hostname wazuh_manager
lsb_release -a
sudo apt update && sudo apt upgrade -y

# --- Download and Configure Wazuh Installer ---
wget <wazuh-install.sh_url>
wget <config.yml_url>
nano config.yml
sudo bash wazuh-install.sh -c config.yml

# --- Indexer Installation & Cluster Setup ---
sudo bash wazuh-install.sh -i
sudo systemctl start wazuh-indexer
curl -u admin:<password> http://<server_ip>:9200

# --- Troubleshooting Installation Issues ---
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys <key>
sudo killall apt apt-get
sudo reboot

# --- Wazuh Manager Installation & Service Management ---
- sudo bash wazuh-install.sh -m
- sudo /var/ossec/bin/wazuh-control status
- systemctl status wazuh-manager
sudo systemctl restart wazuh-manager
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard

# --- Network and Port Checks ---
ss -lntp | grep 55000

# --- API Troubleshooting & Verification ---
curl -k -u 'wazuh-wui:<password>' https://localhost:55000/security/user/authenticate
sudo apt reinstall wazuh-manager

# --- SQLite for API User Management ---
sudo apt install sqlite3
sudo sqlite3 /var/ossec/api/configuration/security/rbac.db
SELECT id, username FROM users;
UPDATE users SET password='PASTE_HASH_HERE' WHERE username='wazuh-ui';

# --- Final Verification ---
# Refresh dashboard in browser and check API status
