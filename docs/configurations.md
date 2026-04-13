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

<h3> Networking Set Up </h3>

```bash
# CD to /etc/network/interfaces

# --- Loopback Interface ---
auto lo
iface lo inet loopback

# --- Physical WiFi Interface (WAN) ---
# Connects the Proxmox Host to the external Gateway
auto wlp0s20f3
iface wlp0s20f3 inet dhcp
    wpa-ssid "${WIFI_SSID}"
    wpa-psk  "${WIFI_PASSWORD}"

# --- Linux Bridge (LAN) ---
# Virtual switch connecting all internal LXC/VM nodes
auto vmbr0
iface vmbr0 inet static
    address ${BRIDGE_IP}/24
    bridge-ports none
    bridge-stp off
    bridge-fd 0

    # 1. IP Forwarding: Enables routing between WAN and LAN
    post-up   echo 1 > /proc/sys/net/ipv4/ip_forward

    # 2. Outbound NAT (Masquerading): 
    # Allows internal nodes to reach the Internet via the Host WiFi
    post-up   iptables -t nat -A POSTROUTING -s '${INTERNAL_SUBNET}' -o wlp0s20f3 -j MASQUERADE
    post-down iptables -t nat -D POSTROUTING -s '${INTERNAL_SUBNET}' -o wlp0s20f3 -j MASQUERADE

    # 3. Inbound DNAT (Port Forwarding): 
    # Maps Host Port 80 to Application Container on internal Port 3000
    post-up   iptables -t nat -A PREROUTING -i wlp0s20f3 -p tcp --dport 80 -j DNAT --to-destination ${WIKI_LXC_IP}:3000
    post-down iptables -t nat -D PREROUTING -i wlp0s20f3 -p tcp --dport 80 -j DNAT --to-destination ${WIKI_LXC_IP}:3000

# --- Unused Physical Interfaces ---
iface nic0 inet manual
iface nic1 inet manual

# --- External Configurations ---
source /etc/network/interfaces.d/*

```

# Wiki.js Container
<h3>  Start Wiki.js Node server on container boot up </h3>

```bash
#Create a new service
sudo nano [PATH_TO_SERVICE]
# Enter the following
[Unit]
Description=Wiki.js
After=network.target postgresql.service

[Service]
Type=simple
User=root
WorkingDirectory=[WORKING_DIR]
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
