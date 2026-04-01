# Configurations
<i> Various code snippets for optimizing this project. </i>


# Proxmox
<h3> Change Sleep Configurations </h3>

```bash
# Loaded & Inactive mean it is able to be used
systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target

# Ensure Sleep status is not enabled
systemctl status sleep.target suspend.target hibernate.target hybrid-sleep.target
```
# Wiki.js Container
<h3>  Start Wiki.js Node server on container boot up </h3>

```bash
#Create a new service
sudo nano /etc/systemd/system/wikijs.service

# Enter the following
[Unit]
Description=Wiki.js
After=network.target postgresql.service

[Service]
Type=simple
User=root
WorkingDirectory=/var/www/wiki
ExecStart=/usr/bin/node server
Restart=always
RestartSec=5
Environment=NODE_ENV=production

[Install]
WantedBy=multi-user.target

# Reload the daemon
systemctl daemon-reload

# Enable the service
systemctl enable wikijs.service

# Reboot
reboot
```
